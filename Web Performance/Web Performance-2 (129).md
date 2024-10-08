# Lighthouse 퍼포먼스 점수 올리기 2: Webpack을 활용한 Preload, Lazy Load, Minify (feat. Vue)

<br>

1. Vue 앱 Preload 설정하기
2. SPA를 빠르게: Webpack의 번들링 원리, 코드 쪼개기(Code Splitting)
3. 동적 임포트: `import()`, 정적 임포트
4. Vue에서 라우트 기반 번들 쪼개기: Webpack 동적 임포트 활용, 번들 네이밍과 그루핑
5. `SplitChunksPlugin`으로 써드파티 모듈 번들링 중복 제거하기
6. `webpack-bundle-analyzer`를 사용하여 JavaScript 번들 분석하기

<br>

## 1. Vue 앱 Preload 설정하기

[Vue](https://vuejs.org/) 앱을 [`vue-cli-service build` 스크립트로 빌드](https://cli.vuejs.org/guide/build-targets.html#app)하면 `dist/` 경로 내에 아래와 같이 앱 번들이 구성됩니다.

- `index.html`
- `js/app.js`
- `js/chunk-vendors.js`
- `css/app.css`
- `css/chunk-vendors.css`
- `image-name.png`

<br>

이 CSS, JavaScript, 이미지 파일들은 다음과 같이 `index.html` 문서에 자동으로 삽입됩니다. 아래 코드에서 알 수 있듯이 초기 앱 렌더링에 필요한 모든 리소스 파일에 `preload`, `modulepreload` 속성이 자동으로 부여됩니다.

```html
<head>
	<link href="/css/app.e39cad8a.css" rel="preload" as="style" />
	<link href="/js/app.39144f4c.js" rel="modulepreload" as="script" />
	<!-- .. -->
</head>
```

<br>

이는 `vue-cli-service build` 명령어가 실행될 때 내부적으로 [`@vue/preload-webpack-plugin`](https://github.com/vuejs/preload-webpack-plugin) 플러그인이 사용되기 때문이고요, Webpack 설정 파일인 `vue.config.js`에서 아래와 같이 Preload 설정을 커스텀할 수 있습니다. 이 예시는 Vue 커뮤니티의 [How to preload CSS in Vue](https://forum.vuejs.org/t/how-to-preload-css-in-vue/90710/8#post_8) 페이지에서 가져왔습니다.

```js
// vue.config.js
module.exports = {
	chainWebpack(config) {
		config.plugins.delete("prefetch");

		config.plugin("preload").tap((options) => {
			options[0].include = "allChunks";
			return options;
		});
	},
};
```

<br>

## 2. SPA를 빠르게: Webpack의 JavaScript 번들링 원리, Webpack의 코드 쪼개기(Code Splitting)

### 2-1. Webpack의 JavaScript 번들링 원리

[CSR(Client Side Rendering)](https://developers.google.com/web/updates/2019/02/rendering-on-the-web#csr)을 기반으로 하는 [SPA](https://developer.mozilla.org/en-US/docs/Glossary/SPA) 방식의 단점 중 하나는 초기 실행 속도가 느리다는 것이었습니다. 앱을 초기 실행하는 시점에 모든 JavaScript 코드가 포함된 거대한 번들(`app.js` 파일 등)을 로드해야하기 때문입니다. 이는 [Webpack](https://webpack.js.org/)과 같은 번들러가 SPA를 빌드하는 방식에 따른 결과입니다. Webpack은 JavaScript 번들을 구성할 때 [`import`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)를 기반으로 모든 [JavaScript 모듈](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)들이 서로 연결되어 있는 의존성 그래프를 만들고요, 이 의존성 그래프에 기반하여 최종 번들에 각 모듈을 포함시킬지 제외할지 결정합니다.

<br>

<img src="../Images/Web Performance/webpack-bundling.png" width="700" />

사진출처 : [Lazy loading and code splitting in Vue.js](https://vueschool.io/articles/vuejs-tutorials/lazy-loading-and-code-splitting-in-vue-js/)

<br>

이렇다보니, 앱의 어떤 [라우트](https://vuejs.org/v2/guide/routing.html)를 요청하던지 모든 JavaScript 모듈과 의존 관계가 포함된 거대한 번들이 로드됩니다. 하지만 각 라우트에서 사용되는 모듈들은 그 중 일부겠죠. 아래와 같이 Chrome 개발자도구의 `Coverage` 탭에서 실제로 사용되지 않는 리소스의 비율을 확인할 수 있습니다. 붉은색으로 표시된 부분이 사용되지 않는 리소스를 나타냅니다.

<br>

<img src="../Images/Web Performance/coverage.png" width="700" />

<br>

### 2-2. Webpack의 코드 쪼개기(Code Splitting)

[Webpack](https://webpack.js.org/)에 [코드 쪼개기(Code Splitting)](https://webpack.js.org/guides/code-splitting/) 기능이 추가되면서 이 문제를 해결할 수 있게 되었습니다. Webpack의 기능을 사용해서 하나의 JavaScript 파일(번들)이 아닌 여러 개의 파일들로 앱을 빌드하고, 각 번들을 필요한 시점에 따로 로드할 수 있게 되었기 때문입니다. 특히 앱의 최초 렌더링에 필요한 JavaScript 코드만이 포함된 번들만 먼저 로드함으로써 FCP를 개선할 수 있게 되었습니다!

<br>

## 3. 동적 임포트: `import()`, 정적 임포트

### 3-1. `import()`

JavaScript 모듈의 기본 임포트 문법은 아래와 같습니다. 이는 정적 임포트 방식입니다. 아래와 같이 `moduleA`를 임포트하면 Webpack의 의존성 그래프에서 `main.js`의 노드 중 하나로 추가되고, 앱의 최종 JavaScript 번들인 `app.js`에 포함되므로 앱의 초기 실행 시 함께 로드됩니다. 만약 앱의 최초 렌더링에 이 모듈이 사용되지 않는다면, 이 모듈을 `app.js` 번들에서 제외시키고 별도의 번들로 묶는 것이 좋겠죠.

```javascript
// main.js
import moduleA from "a";
```

<br>

Webpack의 코드 쪼개기 기능 중 하나인 [동적 임포트](https://webpack.js.org/guides/code-splitting/#dynamic-imports) 문법 `import()`를 사용하면, 초기 렌더링에 사용되지 않는 JavaScript 모듈들을 다른 번들로 분리할 수 있습니다. 아래와 같이 `moduleA`를 동적으로 임포트하면, 이 모듈과 이 모듈에서 의존하는 모듈들 모두 `app.js` 번들에 포함되지 않고 별도의 번들로 분리됩니다. 이 별도로 분리된 [Chunk]() 번들은 앱 초기 실행시 로드되지 않고요, 해당 모듈이 필요해지면 그제서야 "게으르게" 로드됩니다.

```javascript
// main.js
if (isSubmitted) {
	// `import()` 메소드는 `Promise` 객체를 반환한다는 점에 유의하시고요.
	const moduleA = await import("a.moduleA");
	const module = moduleA.default;

	// ..
```

<br>

참고로 [동적 임포트](https://v8.dev/features/dynamic-import) 문법은 [ECMAScript 2020 명세에 포함](https://github.com/tc39/proposal-dynamic-import)되었습니다.

<br>

#### Preload 주석 사용하기

Webpack의 동적 임포트 메소드인 `import()`를 사용할 때, [마법 주석](https://webpack.js.org/api/module-methods/#magic-comments)을 사용하여 특정 모듈만 Preload용 번들에 포함시킬 수 있습니다. 예를 들어, 아래와 같이 `App` 컴포넌트에서 `CriticalComponent` 컴포넌트를 동적으로 임포트할 때 `webpackPreload: true` 주석을 사용하면, `App` 컴포넌트가 로드되는 시점에 `CriticalComponent` 컴포넌트도 함께 요청되어 동시에 로딩이 시작됩니다. [Webpack 공식문서](https://webpack.js.org/guides/code-splitting/)에서 자세한 내용을 확인할 수 있습니다.

```javascript
// App.js
import(/* webpackPreload: true */ "CriticalComponent");
```

<br>

이 기능은 Webpack 4.6.0 버전부터 지원합니다. 이전 버전의 Webpack을 사용해야한다면 [preload-webpack-plugin](https://github.com/GoogleChromeLabs/preload-webpack-plugin) 플러그인을 사용할 수 있습니다.

<br>


## 4. Vue에서 라우트 기반 번들 쪼개기: Webpack 동적 임포트 활용, 번들 네이밍과 그루핑

### 4-1. Webpack 동적 임포트 활용

이번에는 Vue 앱의 컴포넌트들을 라우트별로 동적 로드하는 방법을 소개합니다. 일명 라우트 기반 번들 쪼개기인데요, 사용자가 특정 URL에 대해 라우팅을 요청하면 해당 라우트에 필요한 컴포넌트만 로드할 수 있게 하는겁니다. 위에서 살펴본 Webpack의 동적 임포트를 활용해서요. 자세한 내용은 공식문서인 [Lazy Loading Routes](https://router.vuejs.org/guide/advanced/lazy-loading.html#grouping-components-in-the-same-chunk)를 참고하세요.

<br>

만약 컴포넌트 임포트가 아래와 같이 작성되어있다면, 컴포넌트들을 정적으로 임포트하고 있는겁니다. 실제로 사용자가 `/about` 경로에 접근하지 않더라도, `About` 컴포넌트는 앱이 실행될 때 무조건 로드됩니다.

```javascript
// router.js

import Home from './Home.vue'
import About from './About.vue'

const routes = [
  { path: '/', component: Home }
  { path: '/about', component: About }
]
```

<br>

위 코드는 아래와 같이 Webpack의 `import()` 문법을 사용하여 바꿀 수 있습니다. 이제 사용자가 `/about` 경로에 접근했기 때문에 `About` 컴포넌트가 실제로 사용되어야 할 때만 `About` 컴포넌트가 포함된 번들이 로드됩니다.

```javascript
// router.js

const Home = () => import('./Home.vue')
const About = () => import('./About.vue')

const routes = [
  { path: '/', component: Home }
  { path: '/about', component: About }
]
```

<br>

### 4-2. 번들 네이밍과 그루핑

만약 [`webpackChunkName`](https://webpack.js.org/api/module-methods/#magic-comments) 주석을 사용하면, 해당 모듈이 포함된 번들에 원하는 이름을 부여하고 다른 번들로부터 분리할 수 있습니다. 아래와 같이 두 컴포넌트에 `home`이라고 지정하면, `Home`과 `About` 컴포넌트는 `home.js` 번들에 함께 포함됩니다.

```javascript
const Home = () => import(/* webpackChunkName: "home" */ "./Home.vue");
const About = () => import(/* webpackChunkName: "home" */ "./About.vue");
```

<br>

## 5. `SplitChunksPlugin`으로 써드파티 모듈 번들링 중복 제거하기

### 5-1. `SplitChunksPlugin`

앱을 빌드하면 메인 번들인 `app.js`와 별도로 `chunk-vendors.js` 번들이 생깁니다. 이 번들은 `node_modules`에 포함된 모든 써드파티 라이브러리 모듈들을 담고 있습니다. 예를 들어 [`moment`](https://momentjs.com/) 모듈을 2 개의 라우트에서 사용하고, 각 라우트별로 번들을 구성한다고 가정해보겠습니다. 이러한 경우 `moment` 모듈은 2 개의 번들에 중복 존재하게 됩니다! Webpack 4부터 지원하는 [`SplitChunksPlugin`](https://webpack.js.org/plugins/split-chunks-plugin/)은 써드파티 모듈의 중복제거를 자동으로 수행합니다.

<br>

Vue 앱의 Webpack 설정 파일인 `vue.config.js` 파일을 사용한다면 아래와 같이 `configureWebpack` 필드에서 설정해줄 수 있습니다. 설정하지 않더라도 아래와 같은 디폴트 설정값이 반영되므로 참고해주시고요, 변동사항이 있을 수 있으므로 [공식문서](https://webpack.js.org/plugins/split-chunks-plugin/#optimizationsplitchunks)를 확인해주세요.

```javascript
// vue.config.js

module.exports = {
	// ..
	configureWebpack: {
		optimization: {
			splitChunks: {
				chunks: 'async',
				minSize: 20000,
				minRemainingSize: 0,
				minChunks: 1,
				maxAsyncRequests: 30,
				maxInitialRequests: 30,
				enforceSizeThreshold: 50000,
				cacheGroups: {
					defaultVendors: {
					test: /[\\/]node_modules[\\/]/,
					priority: -10,
					reuseExistingChunk: true,
					},
					default: {
					minChunks: 2,
					priority: -20,
					reuseExistingChunk: true,
					},
				},
			},
		},
	},
};
```

<br>

### 5-2. `splitChunks.chunks`

보통은 `splitChunks.chunks` 속성을 건드리는 것으로 충분합니다. 설정값에 따라 각각 어떻게 번들링 최적화(중복제거)가 이루어지는지 설명하기 위해 간단한 예제를 가져왔습니다. 다음과 같이 `static-module`을 정적 임포트하고, `dynamic-module` 모듈을 동적으로 임포트하는 앱이 있다고 가정해보겠습니다.

```javascript
// app.js
import staticModule from 'static-module';

async function getDynamicModule() {
	await import('dynamic-module')
}

getDynamicModule()
```

<br>

다음은 각 설정값에 따른 번들링 결과물을 간단히 나타낸 것입니다. 여기에서 `async`는 비동기 방식으로 동적 임포트하는 모듈을 의미합니다.

`async` (디폴트)

- `bundle.js` (`app.js` + `static-module`)
- `chunk.js` (`dynamic-module`)

`initial`

- `app.js`
- `bundle.js` (`static-module`)
- `chunk.js` (`dynamic-module`)

`all`

- `app.js`
- `bundle.js` (`static-module` + `dynamic-module`)

<br>

`all`은 정적 임포트를 했든, 동적 임포트를 했든 모든 써드파티 모듈들을 모아서 공통 의존성이 있는 경우 중복을 제거하겠다는 의미입니다. 결과적으로 총 번들 사이즈를 가장 많이 줄일 수 있죠. 다음은 [공식문서](https://webpack.js.org/plugins/split-chunks-plugin/#splitchunkschunks)의 설명입니다.

> Providing all can be particularly powerful, because it means that chunks can be shared even between async and non-async chunks.

<br>

```javascript
// vue.config.js

module.exports = {
	// ..
	configureWebpack: {
		optimization: {
			splitChunks: {
				chunks: "all",
			},
		},
	},
};
```

<br>

### 5-3. `splitChunks.automaticNameDelimiter` (번들 네이밍)

가령 `moment` 라이브러리 모듈을 `/home`, `/about` 2 개의 라우트에서 공통으로 사용한다고 해봅시다. 추가로, 각 라우트에 대한 뷰 컴포넌트는 동적으로 임포트되며 `webpackChunkName` 주석을 사용하여 번들명을 각각 `home`, `about`으로 설정한다고 가정하겠습니다. 이제 `moment` 모듈이 포함된 번들은 아래와 같이 자동으로 네이밍됩니다.

- `home~about.js`

<br>

`splitChunks.automaticNameDelimiter` 속성은 위 번들명에서 `~`에 해당하는 부분에 관여합니다. 이 속성의 디폴트 값은 `~`입니다.

<br>

## 6. `webpack-bundle-analyzer`를 사용하여 JavaScript 번들 분석하기

[`webpack-bundle-analyzer`](https://github.com/webpack-contrib/webpack-bundle-analyzer)는 앱 번들 결과물에서 어떤 모듈이 어느정도 비중을 차지하는지 분석하고 시각화해서 보여주는 라이브러리입니다. 아래와 같이 `webpack-bundle-analyzer`를 개발용으로 설치하시고요,

```zsh
yarn add -D webpack-bundle-analyzer
```

<br>

Vue의 Webpack 설정 파일인 `vue.config.js`에서 플러그인으로 추가합니다.

```javascript
// vue.config.js
const BundleAnalyzerPlugin =
	require("webpack-bundle-analyzer").BundleAnalyzerPlugin;

module.exports = {
	// ..
	plugins: [new BundleAnalyzerPlugin()],
};
```

<br>

이제 `vue-cli-service build`를 실행시켜 앱을 빌드하면, 다음과 같이 분석 결과를 확인할 수 있습니다.

<img src="../Images/Web Performance/webpack-bundle-analyzer.png" />

<br>

---

### References

- [Webpack and Dynamic Imports: Doing it Right](https://medium.com/front-end-weekly/webpack-and-dynamic-imports-doing-it-right-72549ff49234)
- [Lazy loading and code splitting in Vue.js](https://vueschool.io/articles/vuejs-tutorials/lazy-loading-and-code-splitting-in-vue-js/)
- [Vue.js Router Performance](https://vueschool.io/articles/vuejs-tutorials/vue-js-router-performance/)
- [Lazy Loading Routes | Vue Router](https://router.vuejs.org/guide/advanced/lazy-loading.html#grouping-components-in-the-same-chunk)
- [How to Reduce Your Vue.JS Bundle Size With Webpack - Jennifer Bland](https://medium.com/js-dojo/how-to-reduce-your-vue-js-bundle-size-with-webpack-3145bf5019b7)
- [Code Splitting With Vue.js And Webpack - Anthony Gore](https://vuejsdevelopers.com/2017/07/03/vue-js-code-splitting-webpack/)
- [3 Code Splitting Patterns For VueJS and Webpack - Anthony Gore](https://vuejsdevelopers.com/2017/07/08/vue-js-3-ways-code-splitting-webpack/)
- [Webpack: What is the difference between "all" and "initial" options in optimization.splitChunks.chunks | Stack Overflow](https://stackoverflow.com/questions/50127185/webpack-what-is-the-difference-between-all-and-initial-options-in-optimizat)
- [The 100% correct way to split your chunks with Webpack](https://medium.com/hackernoon/the-100-correct-way-to-split-your-chunks-with-webpack-f8a9df5b7758)
- [SPA 초기 로딩 속도 개선하기](https://medium.com/little-big-programming/spa-%EC%B4%88%EA%B8%B0-%EB%A1%9C%EB%94%A9-%EC%86%8D%EB%8F%84-%EA%B0%9C%EC%84%A0%ED%95%98%EA%B8%B0-9db137d25566)
- [웹팩5(Webpack) 설정하기 | zerocho.com](https://www.zerocho.com/category/Webpack/post/58aa916d745ca90018e5301d)