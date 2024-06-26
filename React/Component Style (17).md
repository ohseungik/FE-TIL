# 컴포넌트 스타일링

<br>

## CRA sass-loader 설정 커스터마이징

<br>

리액트 파일에서 sass 파일을 불러올때

sass파일을 불러오는 과정이 복잡할수도있다.

<br>

때문에 필수는 아니지만 유용한 방법으로 sass-loader을 커스터마이징을 하여

한번에 찾아올 수 있게해준다.

```jsx
@import '../../../styles/utils'
```

<br>

CRA에서 이미 설치되어있는 웹팩의 sass-loader을 커스터마이징 하면된다.

웹팩에서 세부설정들이 숨겨져있다.

떄문에 명령어를 통해서 세부설정을 밖으로 꺼내야한다.

```bash
$ npm run eject
```

<br>

config라는 디렉터리가 생긴곳에서

webpack.config.js를 열어 test: 'sassRegex' 인부분을 수정해준다.

```jsx
{
	test: sassRegex,
	exclude: sassModuleRegex,
	use: getStyleLoaders({
	importLoaders: 2,
	sourceMap: isEnvProduction && shouldUseSourceMap,
		}).concat({
			loader: require.resolve("sass-loader"),
			options: {
				sassOptions: {
				includePaths: [paths.appSrc + "/styles"],
			},
			sourceMap: isEnvProduction && shouldUseSourceMap,
		},
	}),
	sideEffects: true,
},
```

<br>

이제 utils.scss파일을 부르면 앞의상대경로를 입력안해도

styles 디렉토리를 기준으로 경로를 불러올수있다.

<br>

하지만 파일을 생성할떄마다 utils.scss를 불러오는것은 불편하다.

data를 설정해주면 불러오기를 안해도 자동으로 불러와준다.

```jsx

{
	test: sassRegex,
	exclude: sassModuleRegex,
	use: getStyleLoaders({
	importLoaders: 2,
	sourceMap: isEnvProduction && shouldUseSourceMap,
		}).concat({
			loader: require.resolve("sass-loader"),
			options: {
				sassOptions: {
				includePaths: [paths.appSrc + "/styles"],
			},
			sourceMap: isEnvProduction && shouldUseSourceMap,
			prependData: `@import 'utils';`,
		},
	}),
	sideEffects: true,
},

```

<br>

### node_modules 라이브러리 쉽게 불러오기

Sass에서는 라이브러리를 쉽게 불러와서 사용할수 있다.

node_modules까지 직접 경로를 입력해서 불러오지 않아도 되고

```jsx
@import '../../../node_modules/library/...'
```

<br>

물결표를 사용해서 node_modules로 가는 경로를 생략할수있다.

```jsx
@import '~library/styles'
```

<br>

## CSS Module

<br>

React에서 CSS파일을 불러와서 적용시킬때

만약 공통된 클래스의 CSS을 적용시킬수도있다는 단점이있다.

<br>

때문에 CSS Module을 사용하면 파일을 import하고 CSS을 적용시키면

[ 파일이름 ]_[ 클래스 이름 ]_[ 해시값 ] 형태로 자동으로 컴포넌트 스타일 클래스 이름을 가져 충접되는 현상을 방지해준다.

<br>

.module.css 확장자로 저장만 하면 CSS Module이 적용된다.

때문에 CSS Module을 사용하면 클래스 이름을 지을때 중복되는것에 대해 고민을 하지 않아도 된다.

<br>

만약 웹페이지 전역적으로 사용되어진다면

:globla을 붙이면된다.

```css
:global {
  // :global {}로 감싸기
  .something {
    font-weight: 800;
    color: aqua;
  }
```

styles가 객체로 키-값형태로 저장이 되어있다.

때문에 객체 형식처럼 styles.wrapper 처럼 불러와야한다.

<br>

글로벌은 일반적으로 넣었던방식대로 하면된다.

```css
import React from 'react';
import styles from './CSSModule.module.css';

const CSSModule = () => {
  return (
    <div className={styles.wrapper}>
      안녕하세요
    </div>
  );
};

export default CSSModule;
```

<br>

만약 두개 이상의 클래스를 적용시킬때는

템플릿 리터럴도 적용하면된다.

```css
import React from 'react';
import styles from './CSSModule.module.css';

const CSSModule = () => {
  return (
    <div className={`${styles.wrapper} ${styles.inverted}`}>
      안녕하세요
    </div>
  );
};

export default CSSModule;
```

<br>

### classnames

여러클래스를 적용할때 탬플릿 리터럴 불편하게 적용해야한다.

때문에 라이브러리중에 classnames를 사용하면

<br>

1. 여러클래스를 적용할때 편리
2. 클래스를 조건부로 설정할때 편리하다.

<br>

라이브러리설치

```css
npm i classnames
```

<br>

classnaems라이브러리에서 import를 해와

classnames함수안에 인수로 클래스를 넣어주면된다.

```jsx
import classNames from "classnames";
import React from "react";
import styles from "./CSSModule.module.css";

const CSSModule = () => {
  return (
    <div className={classNames("styles.wrapper", "styles.inverted")}>
      안녕하세요
    </div>
  );
};

export default CSSModule;
```

<br>

매번 styles.을 넣어주어야하므로

bind함수를 사용하면 styles.을 생략해주어도된다.

```jsx
import classNames from "classnames/bind";
import React from "react";
import styles from "./CSSModule.module.css";

const cx = classNames.bind(styles);

const CSSModule = () => {
  return <div className={cx("wrapper", "inverted")}>안녕하세요</div>;
};

export default CSSModule;
```

<br>

Sass도 마찬가지로 .module.scss만 붙여주면 가능하다.

<br>