# 배열(Array)과 배열형 객체

## 목차

```
1. 배열(Array)과 배열형 객체
2. Array 객체
3. JavaScript 배열의 특징
4. 배열 생성하기
5. 리터럴 vs 생성자
6. 배열 판별하기
7. Array 인스턴스 속성
8. Array 인스턴스 메소드
9. 변경자 메소드
10. 접근자 메소드
11. 순회 메소드
```

<br>

## 1. 배열(Array)과 배열형 객체

아래는 [자바스크립트 표준 내장 객체](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects) 중에서 인덱스 컬렉션(Indexed Collections)에 해당하는 객체들입니다. 인덱스 값으로 정렬된 데이터의 컬렉션(집합)을 나타내고요, 배열([형식화 배열(Typed Array)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Typed_arrays) 포함)과 배열형 객체를 포함합니다.

- `Array`

- `Int8Array`

- `Uint8Array`

- `Uint8ClampedArray`

- `Int16Array`

- `Uint16Array`

- `Int32Array`

- `Uint32Array`

- `Float32Array`

- `Float64Array`

- `BigInt64Array`

- `BigUint64Array`

<br>

## 2. `Array` 객체

`Array`는 리스트 형태의 객체입니다. JavaScript에는 명시적인 배열(Array) 자료형이 없기 때문에 배열을 다루기 위해서는 `Array` 객체를 사용해야 합니다. 배열을 생성할 때 사용할 수 있고요, `Array.prototype`은 배열을 탐색하고 변형시키는 메소드들을 제공합니다.

<br>

### 생성자

- [`Array()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Array) : `Array` 객체를 생성합니다.

모든 `Array` 인스턴스는 `Array.prototype`을 상속합니다. 다른 생성자와 마찬가지로, `Array()` 생성자의 프로토타입을 수정하면 모든 `Array` 인스턴스도 영향을 받습니다. 예를 들면, 새로운 메소드와 속성을 추가해 모든 `Array`를 확장할 수 있으므로, [폴리필(Polyfill)](https://developer.mozilla.org/en-US/docs/Glossary/Polyfill)에 쓰입니다.

그러나 배열 객체에 비표준 메소드를 추가하면 나중에 스스로, 혹은 JavaScript에 기능이 추가될 경우 문제가 될 수 있습니다.

<br>

### 정적 속성

- `Array.length` : 값이 `1`인 Array 생성자의 길이 속성입니다.

- [`get Array[@@species]`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/@@species) : 파생 객체를 생성하는데 사용하는 생성자 함수입니다.

<br>

### 정적 메소드

- [`Array.from()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from)

- [`Array.isArray()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/isArray)

- [`Array.of()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/of)

<br>

#### `Array.from()`

유사 배열(array-like) 또는 반복 가능한 객체(iterable object)로부터 새로운 `Array` 인스턴스를 생성합니다. 다음과 같은 경우에 `Array.from()` 메소드로 새 `Array` 객체를 만들 수 있습니다.

- 유사 배열 객체 (`length` 속성과 인덱싱된 요소를 가진 객체)
- [반복 가능한 객체](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols) ([Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map), [Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set) 등 객체의 요소를 얻을 수 있는 객체)

<br>

#### `Array.isArray()`

배열이면 `true`, 아니면 `false`를 반환합니다.

<br>

#### `Array.of()`

전달인자의 개수나 데이터 타입에 관계없이 새 `Array` 인스턴스를 생성합니다. `Array.of()`와 `Array()` 생성자의 차이는 인자 1개를 전달했을 때의 처리 방법에 있습니다. `Array.of(7)`은 하나의 요소 `7`을 가진 배열을 생성하지만 `Array(7)`은 `length`가 `7`인 빈 배열을 생성합니다.

```javascript
Array.of(7); // [7]
Array(7); // [ , , , , , , ]
```

<br>

## 3. JavaScript 배열의 특징

JavaScript의 배열은 아래와 같은 특징을 가집니다.

- 배열은 객체입니다.

- 길이가 고정되어 있지 않습니다.

- 배열 원소들의 타입이 고정되어있지 않습니다.

- 배열의 인덱스 값은 `0`부터 시작하는 정수입니다.

<br>

### 배열은 객체입니다

아래와 같이, 배열도 객체이므로 `typeof arr`의 반환값은 `object` 입니다.

```javascript
const arr = [1, 2, 3];
console.log(typeof arr); // output : object
console.log(arr.constructor === Array); // output : true
```

<br>

### 길이가 고정되어 있지 않습니다

배열의 길이가 언제든 인위적으로 변경될 수 있기 때문에 배열의 길이와 실제 원소의 수가 일치하지 않을 수 있습니다. 아래와 같은 배열을 만들었다고 가정해보세요. 실제 존재하는 데이터는 5 개이지만 이 배열의 길이는 `9`입니다. 아래 예제와 같이 원소가 연속적으로 밀집되어있지 않은 배열을 [희소 배열(Sparse Array)](https://www.oreilly.com/library/view/javascript-the-definitive/9781449393854/ch07s03.html)이라고 합니다. 이처럼 JavaScript의 배열은 밀집도를 보장하지 않습니다. 이러한 특징이 적합하지 않은 상황에서는 형식화 배열(Typed array)를 사용하세요.

```javascript
const arr = [1, 2, 3, 4, , 5, , ,];
console.log(arr.length); // output : 9
```

<br>

배열의 길이를 짧게 지정하여 원소를 제거할 수도 있습니다. 이번에는 아래와 같은 밀집 배열(Dense Array)이 있다고 상상해보세요.

```javascript
const arr2 = [1, 2, 3, 4, 5, 6, 7, 8];
console.log(arr2.length); // 8
```

<br>

이제 이 배열의 길이를 `3`으로 변경해볼겁니다.

```javascript
arr2.length = 3;
console.log(arr2); // [1, 2, 3]
```

어떤가요? 배열의 길이가 (당연히) `3`이 되었고요, 이 길이를 넘어서는 원소들이 자동으로 제거되었습니다.

<br>

### 배열 원소들의 타입이 고정되어있지 않습니다

아래와 같이 하나의 배열에 서로 다른 타입을 가진 원소들을 담을 수 있습니다. 배열의 각 원소에는 거의 모든 것을 저장할 수 있습니다. 문자열, 숫자, 객체, 다른 변수, .., 심지어 다른 배열도요.

```javascript
const arr2 = [7, "Hello", true, ["a", "b", "c"]];
```

<br>

#### 다중배열

배열 내부의 배열은 다중배열이라고 하는데요, 대괄호 두개를 함께 연결하여 다른 배열 안에 있는 배열의 원소에 접근 할 수 있습니다. 위의 `arr2` 배열 내부에 속해있는 배열 `["a", "b", "c"]`의 마지막 원소인 `"c"`에 접근하려면 다음과 같은 방법이 유효합니다.

```javascript
const c = arr2[3][2];
console.log(c); // output: "c"
```

<br>

### 배열의 인덱스 값은 `0`부터 시작하는 정수입니다.

말그대로 배열의 인덱스 값은 `0`부터 시작하는 정수입니다. 희소 배열의 경우, 값이 없는 원소에도 인덱스를 사용해서 접근할 수 있습니다. 물론 그 값은 `undefined` 입니다.

```javascript
const arr = [1, 1, , , , , , 4];
console.log(arr[3]); // undefined
```

<br>

## 4. 배열 생성하기

배열을 생성하는 방법에는 두 가지가 있습니다.

- [배열 리터럴(Array literals)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#Array_literals)

- [`Array()` 생성자 함수](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Array)

<br>

### 배열 리터럴 표기법

아래와 같이 배열을 생성하면 배열 리터럴 표기법을 사용한 것입니다. 리터럴 표기법으로 배열을 생성하면 명시한 값들로 배열이 초기화되고요, 명시된 원소들의 수가 해당 배열의 길이가 됩니다. 네, 필요한 원소들을 열거하는 것만으로 배열 객체가 생성됩니다.

```javascript
let cities = ["Seoul", "New York", "Barcelona"];
```

> 리터럴(Literal)은 변수에 할당하는 고정 형태의 값을 말합니다. 작성한 문자 그대로 자신을 나타내는 값이 됩니다.

<br>

### `Array()` 생성자 함수

`Array()`는 생성자 함수입니다. 배열, 그러니까 `Array` 객체의 생성과 초기화를 담당하는 함수죠.

```javascript
let a = new Array(); // []
```

<br>

#### 하나의 인자를 넘겨줄 때

하나의 숫자(정수) 인자를 넘겨주면 그 숫자 만큼의 길이를 가지는 배열이 생성됩니다. 하지만 이렇게 생성된 배열은 원소가 없는 빈 배열입니다. 원소가 존재하지 않기 때문에 어느 원소에 접근하든 반환값은 `undefined` 입니다. 이때 배열의 빈 공간들은 [`fill()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/fill) 메소드로 채울 수 있습니다...

```javascript
let b = new Array(3); // [empty × 3]
console.log(b[0]); // undefined
console.log(b.length); // 3
```

<br>

인자를 하나만 넘겨주어 특정 길이를 가진 배열을 만들 때 주의할 점은 정수만 가능하다는 것입니다. 아래와 같이 부동소수점을 가진 숫자를 전달하면 에러가 발생합니다.

```javascript
const c = new Array(3.4); // RangeError: Invalid array length
```

<br>

> 배열의 길이를 지정할 수 있다는 점과 [`keys()`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/keys) 메소드를 이용하면 아래와 같은 트릭이 가능합니다.

```javascript
const d = [...new Array(10).keys()];
console.log(d); // output : [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

<br>

#### 2 개 이상의 인자를 넘겨줄 때

2 개 이상의 인자를 넘겨주면 각 인자가 생성된 배열의 원소들로 초기화됩니다. 넘겨준 인자의 수가 배열의 길이가 됩니다.

```javascript
let e = new Array(1, 2); // [1, 2]
```

<br>

## 5. 리터럴 vs 생성자

리터럴과 생성자 함수. 둘 중 어느 방법이 더 좋은가요? 이 질문에 대해서는 널리 알려진 가이드(?)가 있습니다. [Oreilly사의 JavaScript Patterns](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=13680905)에서 소개된 내용이고, 이 책의 내용을 레퍼런스로 하는 블로그 글과 아티클도 많은 것 같습니다.

결론적으로, 리터럴 패턴을 사용하는 것이 (보통은) 더 좋습니다. 많은 사람들이 이에 동의하는 근거는 다음과 같습니다.

- 리터럴 패턴이 더 직관적이고 코드도 깔끔합니다.

```javascript
// 생성자
let arr = new Array();

// 리터럴
let arr = [];
```

<br>

- 생성자 함수는 인자를 받을 수 있는데, 이로 인해 예상 밖의 결과가 발생합니다.

```
위에서 살펴봤듯이, 인자를 하나만 전달하면 배열의 길이가 되고, 2 개 이상을 전달하면 각각이 배열의 원소가 되고요..
인자를 하나만 넘기는 경우 정수가 아닌 숫자를 전달하면 에러가 발생한다던가.. 아무튼 여러모로 신경쓸 것이 많고 비효율적입니다.
```

<br>

한 줄로 요약하자면, 리터럴 패턴의 표현이 더 명확하고 코드도 깔끔하면서 에러 발생의 가능성이 낮기 때문에 더 좋습니다.

<br>

## 6. 배열 판별하기

배열인지 판별할 수 있는 가장 확실한 방법은 `isArray()` 메소드를 사용하는 것입니다.

<br>

### `Array.isArray()`

[`Array.isArray()`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/isarray) 메소드를 사용하세요. 이 메소드는 인자가 배열이면 `true`를 반환합니다.

```javascript
const arr = [];
Array.isArray(arr); // true

const obj = {
	length: 1,
	0: 1,
};
Array.isArray(obj); // false
```

<br>

### `Object.prototype.toString()`

ES5의 `Array.isArray()` 메소드를 사용할 수 없는 구형 브라우저 환경을 지원해야 한다면 사용하세요. [`Object.prototype.toString()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/toString) 메소드를 호출하는 방법입니다.

```javascript
const arr = [];
const type = Object.prototype.toString.call(arr);
console.log(type); // "[object Array]"
```

> MDN 문서에 따르면, 기본적으로 `toString()` 메소드는 `Object`에서 비롯된 모든 객체에 상속됩니다. 이 메소드가 사용자 지정 개체에서 재정의되지 않으면 `toString()`은 `"[object type]"`을 반환합니다. 여기서 `type`은 `object type`입니다.

> `Function.prototype.call()`에 대한 [MDN 문서](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call)를 확인하세요.

<br>

위의 예제를 응용하면 아래와 같이 `Array.isArray()` 메소드를 정의해서 사용할 수 있겠죠.

```javascript
if (typeof Array.isArray === undefined) {
	Array.isArray = function (obj) {
		return Object.prototype.toString.call(obj) === "[object Array]";
	};
}
```

<br>

사실 이 방법도 100% 완벽하지는 않습니다. ES6+ 에서 `Symbol.toStringTag`를 사용하면 객체 타입을 조작(?)할 수 있기 때문이죠. 어쨋든 IE8 이하의 구형 브라우저를 지원해야만 한다면 플랜B 정도로 알아두면 나쁘지 않을 것 같습니다.

```javascript
class ArrayClass {
	get [Symbol.toStringTag]() {
		return "Array";
	}
}

console.log(Object.prototype.toString.call(new ArrayClass())); // "[object Array]"
```

<br>

### 쓸모없거나 좋지 못한 방법들

- 배열에 `typeof` 연산자를 사용하면 `object`가 반환됩니다. 위에서 언급했듯이, JavaScript에서는 배열도 객체이기 때문이죠.

```javascript
console.log(typeof []); // output : object
```

<br>

- [`instanceof`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/instanceof) 연산자를 사용하는 것도 방법인 것처럼 보입니다만, 완벽하지 않습니다. 이 방법은 프레임에서 사용시 배열을 판별하지 못합니다.

```javascript
const FrameArray = window.frames[1].Array;
const arr = new FrameArray(1, 1, 1);

console.log(Array.isArray(arr)); // true
console.log(arr instanceof Array); // false
```

<br>

> StackOverFlow에서 [Difference between using Array.isArray and instanceof Array](https://stackoverflow.com/questions/22289727/difference-between-using-array-isarray-and-instanceof-array/22289869)를 주제로 한 논의가 있고요, 지금 이 글을 작성하는데에도 많은 도움이 되었습니다.

<br>

## 7. `Array` 인스턴스 속성

- `Array.prototype.constructor` : 객체의 프로토타입을 생성하는 함수를 지정합니다.

- `Array.prototype.length` : 배열의 원소 수를 나타냅니다.

- `Array.prototype[@@unscopables]` : `with` 결속 범위로부터 제외하려는 속성 이름이 들어있는 기호입니다.

<br>

## 8. `Array` 인스턴스 메소드

### 변경자 메소드

아래의 변경자 메소드들은 배열을 수정합니다.

<br>

- [`copyWithin()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/copyWithin) : 배열의 일부를 얕게 복사한 뒤, 동일한 배열의 다른 위치에 덮어쓰고 그 배열을 반환합니다.

- [`fill()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/fill) : 배열의 시작 인덱스부터 끝 인덱스의 이전까지 정적인 값 하나로 채웁니다.

- [`pop()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/pop) : 배열에서 마지막 요소를 제거하고 그 요소를 반환합니다.

- [`shift()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/shift) : 배열에서 첫 번째 요소를 제거하고, 제거된 요소를 반환합니다.

- [`unshift()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/unshift) : 새로운 요소를 배열의 맨 앞쪽에 추가하고, 새로운 길이를 반환합니다.

- [`push()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push) : 배열의 끝에 하나 이상의 요소를 추가하고, 배열의 새로운 길이를 반환합니다.

- [`reverse()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse) : 배열의 순서를 반전합니다.

- [`sort()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort) : 배열의 요소를 적절한 위치에 정렬한 후 그 배열을 반환합니다. 정렬은 [stable sort](https://en.wikipedia.org/wiki/Sorting_algorithm#Stability)가 아닐 수 있습니다. 기본 정렬 순서는 문자열의 유니코드 코드 포인트를 따릅니다.

- [`splice()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice) : 배열의 기존 요소를 삭제 또는 교체하거나 새 요소를 추가하여 배열의 내용을 변경합니다.

<br>

### 접근자 메소드

아래의 접근자 메소드들은 배열을 수정하지 않고, 기존 배열의 일부에 기반한 새로운 배열 또는 값을 반환합니다.

<br>

- [`concat()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat) : 인자로 주어진 배열이나 값들을 기존 배열에 합쳐서 새 배열을 반환합니다.

- [`filter()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) : 주어진 함수의 테스트를 통과하는 요소들만 모아 새로운 배열로 반환합니다.

- [`includes()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/includes) : 배열이 특정 요소를 포함하고 있는지 판별합니다. `true`/`false`를 반환합니다.

- [`indexOf()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf) : 배열에서 지정된 요소를 찾을 수 있는 첫 번째 인덱스를 반환하고 존재하지 않으면 `-1`을 반환합니다.

- [`lastIndexOf()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/lastIndexOf) : 배열에서 주어진 값을 발견할 수 있는 마지막 인덱스를 반환하고, 요소가 존재하지 않으면 `-1`을 반환합니다.

- [`join()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join) : 인자로 전달된 문자열을 이용하여 배열의 모든 요소를 연결해 하나의 문자열로 만듭니다.

- [`slice()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice) : 어떤 배열의 시작 인덱스부터 마지막 인덱스까지(마지막 인덱스의 원소는 미포함)에 대한 얕은 복사본을 새로운 배열 객체로 반환합니다.

- [`toString()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toString) : 지정된 배열 및 그 요소를 나타내는 문자열을 반환합니다. 각 원소는 `,`로 연결됩니다. `Object.prototype.toString()` 메소드를 재정의합니다.

- [`toLocaleString()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toLocaleString) : 배열의 요소를 나타내는 문자열을 반환하는데, `Date` 타입의 요소는 지역화된 문자열을 반환합니다. `Object.prototype.toLocaleString()` 메소드를 재정의합니다.

<br>

### 순회 메소드

순회 메소드를 호출하면 배열의 `length`를 기억하므로, 아직 순회가 끝나지 않았을 때 요소를 더 추가하더라도 콜백이 호출되지 않습니다. 반드시 배열을 변형해야 한다면, 새로운 배열로 복사하세요.

<br>

- [`forEach()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) : 주어진 함수를 배열 요소 각각에 대해 실행합니다.

- [`map()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) : 배열 내의 모든 요소 각각에 대하여 주어진 함수를 호출한 결과(반환값)을 모아 새로운 배열을 반환합니다.

- [`reduce()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce) : 배열의 각 요소에 대해 주어진 리듀서(reducer) 함수를 실행하고, 결과값을 반환합니다. 리듀서 함수는 인자로 이전까지의 누적값과 현재값(`(accumulator, currentValue)`)를 받습니다.

- [`reduceRight()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/ReduceRight) : 리듀서의 첫 번째 인자인 누적값에 대해 함수를 적용하고, 두 번째 인자인 현재값은 배열의 오른쪽에서 왼쪽(마지막에서 첫 번째 요소) 순으로 구성됩니다.

- [`some()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/some) : 배열 안의 어떤 요소라도 주어진 판별 함수를 통과하면 `true`, 모든 요소가 통과하지 못하면 `false`를 반환합니다. 빈 배열에서 호출하면 무조건 `false`를 반환합니다.

- [`every()`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/every) : 배열 안의 모든 요소가 주어진 판별 함수를 통과하면 `true`, 하나라도 통과하지 못하면 `false`를 반환합니다. 빈 배열에서 호출하면 무조건 `true`를 반환합니다.

- [`find()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find) : 주어진 판별 함수를 만족하는 첫 번째 요소의 값을 반환합니다. 그런 요소가 없다면 `undefined`를 반환합니다.

- [`findIndex()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex) : 주어진 판별 함수를 만족하는 배열의 첫 번째 요소에 대한 인덱스를 반환합니다. 만족하는 요소가 없으면 `-1`을 반환합니다.

- [`keys()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/keys) : 배열의 각 인덱스를 키 값으로 가지는 새로운 `Array Iterator` 객체를 반환합니다.

- [`entries()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/entries) : 배열의 요소에 대한 "키 - 값" 쌍을 가지는 새로운 `Array Iterator` 객체를 반환합니다.

- [`values()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/values) : 배열의 각 요소에 대한 값을 가지는 새로운 `Array Iterator` 객체를 반환합니다.

- [`Array.prototype[Symbol.iterator]()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/@@iterator) : 기본값은 `values()` 입니다.

<br>

## 9. 변경자 메소드

변경자 메소드를 호출하면 배열이 수정됩니다. 배열의 원본을 유지해야한다면 주의하세요.

<br>

### [`copyWithin()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/copyWithin)

배열의 일부를 얕게 복사한 뒤, 동일한 배열의 다른 위치에 덮어쓰고 그 배열을 반환합니다. 이때 해당 배열의 길이를 바꾸지 않는다는 것에 주의하세요. 그러니까, 복사한 원소의 수만큼 다른 위치의 원소들을 덮어씁니다. 아래의 예제를 보죠. 이 메소드는 변경자 메소드이기 때문에(배열을 직접 수정하기 때문에) 원래 배열인 `arr`과 메소드의 반환 값이 동일하다는 점에 주목하세요.

<br>

```javascript
const arr = [1, 2, 3, 4, 5];
const arr2 = arr.copyWithin(0, 3, 4);

console.log(arr); // [4, 2, 3, 4, 5]
console.log(arr2); // [4, 2, 3, 4, 5]
```

`arr.copyWithin(0, 3, 4)`의 의미는 이렇습니다.

- `0`번 인덱스부터 (복사한 원소들로) 덮어씁니다.
- `3`번 인덱스부터 복사합니다.
- `4`번 인덱스 이전까지 복사합니다. (`4`번 인덱스 미포함)

<br>

> `arr`은 `const`로 선언한 변수이기 때문에 다른 값을 직접 할당해서 변경하는 것은 불가능합니다. 하지만 `Array` 인스턴스 메소드를 사용해서 수정하는 것은 가능합니다.

<br>

세 인자들의 값이 음수이면, 각각 배열의 뒤에서부터 카운트합니다. 예를 들어 첫 번째 인자의 값이 `-2`이면, 배열의 마지막에서 두 번째 원소부터 덮어씁니다.

```javascript
const arr = [1, 2, 3, 4, 5];
arr.copyWithin(-2, 0, 3);

console.log(arr); // [1, 2, 3, 1, 2]
```

<br>

### [`fill()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/fill)

배열의 시작 인덱스부터 끝 인덱스의 이전까지 정적인 값 하나로 채우고 그 배열을 반환합니다.

```javascript
const arr = [1, 2, 3, 4, 5];
const arr2 = arr.fill(0, 1, 5);

console.log(arr); // [1, 0, 0, 0, 0]
console.log(arr2); // [1, 0, 0, 0, 0]
```

<br>

이 메소드의 세 인자를 각각 `value`, `start`, `end`라고 칭해봅시다. 만약 `start`와 `end`의 값으로 음수를 지정하면 그 값은 각각 `start + this.length`, `end + this.length`가 됩니다. 시작과 끝 인덱스를 지정하지 않으면 기본값은 각각 `0`, `this.length` 입니다.

<br>

`value` 값으로 객체를 전달하면, 그 참조만 복사해서 배열을 채웁니다.

```javascript
const arr = new Array(3); // [ empty × 3 ]
arr.fill({}); // [{}, {}, {}]
arr[0].name = "Yujin"; // [{name: "Yujin"}, {name: "Yujin"}, {name: "Yujin"}]
```

<br>

### [`pop()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/pop)

배열에서 마지막 요소를 제거하고 그 요소를 반환합니다. 빈 배열의 경우 `undefined`를 반환합니다.

```javascript
const arr = [1, 2, 3, 4, 5];
const x = arr.pop();

console.log(arr); // [1, 2, 3, 4]
console.log(x); // 5
```

<br>

`pop()` 메소드는 유사배열(array-like) 객체에도 사용할 수 있습니다. 인덱싱된 속성들과 `length` 속성을 가진 유사배열 객체를 만들어보겠습니다.

```javascript
const obj = {
	0: 1,
	1: 2,
	2: 3,
	length: 3,
};

console.log(Array.isArray(obj)); // false
```

<br>

`call()` 또는 `apply()` 메소드를 사용해서 `Array.prototype.pop()` 메소드를 호출해봅니다.

```javascript
const x = Array.prototype.pop.call(obj);

console.log(x); // 3
console.log(obj); // {0: 1, 1: 2, length: 2}
```

<br>

### [`shift()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/shift)

배열에서 첫 번째 요소를 제거하고, 제거된 요소를 반환합니다. `pop()`과 마찬가지로 유사배열 객체에 사용할 수 있습니다.

```javascript
const arr = [1, 2, 3, 4, 5];
const x = arr.shift();

console.log(arr); // [2, 3, 4, 5]
console.log(x); // 1
```

<br>

배열의 길이가 `0`인 경우 `undefined`를 반환합니다. 길이가 있더라도 빈 배열이면 역시나 `undefined`를 반환하고요, 배열의 길이는 수정됩니다.

```javascript
const arr = new Array(3); // [empty × 3]
const x = arr.shift();

console.log(x); // undefined
console.log(arr.length); // 2
```

<br>

### [`unshift()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/unshift)

새로운 요소를 배열의 맨 앞쪽에 추가하고, 새로운 길이를 반환합니다. 유사배열 객체에 사용 가능합니다.

```javascript
const arr = [1, 2, 3];
const length = arr.unshift(4, 5);

console.log(arr); // [4, 5, 1, 2, 3];
console.log(length); // 5
```

<br>

### [`push()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push)

배열의 끝에 하나 이상의 요소를 추가하고, 배열의 새로운 길이를 반환합니다.

```javascript
const arr = [1, 2, 3];
const length = arr.push(4, 5);

console.log(arr); // [1, 2, 3, 4, 5]
console.log(length); // 5
```

<br>

`call()` 또는 `apply()`를 사용하여 유사배열 객체에 사용할 수 있습니다.

```javascript
const obj = {
	0: 1,
	1: 2,
	length: 2,
};

Array.prototype.push.call(obj, 3, 4);
console.log(obj); // {0: 1, 1: 2, 2: 3, 3: 4, length: 4}

Array.prototype.push.apply(obj, [5, 6]);
console.log(obj); // {0: 1, 1: 2, 2: 3, 3: 4, 4: 5, 5: 6, length: 6}
```

<br>

### [`reverse()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)

배열의 순서를 반전하고요,

```javascript
const arr = [1, 2, 3, 4, 5];
const arr2 = arr.reverse();

console.log(arr); // [5, 4, 3, 2, 1]
console.log(arr2); // [5, 4, 3, 2, 1]
```

<br>

원본 배열을 변형하며 그 참조를 반환합니다. 따라서 아래의 `arr.push(0)`은 `arr`과 `arr2`에 모두 영향을 줍니다.

```javascript
arr.push(0);
console.log(arr); // [5, 4, 3, 2, 1, 0]
console.log(arr2); // [5, 4, 3, 2, 1, 0]
```

<br>

### [`sort()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)

배열의 요소를 적절한 위치에 정렬한 후 그 배열을 반환합니다. 정렬은 [stable sort](https://en.wikipedia.org/wiki/Sorting_algorithm#Stability)가 아닐 수 있습니다. 기본 정렬 순서는 문자열의 유니코드 코드 포인트를 따릅니다.

```javascript
const months = ["March", "Jan", "Feb", "Dec"];
months.sort();

console.log(months); // ["Dec", "Feb", "Jan", "March"]
```

<br>

숫자로 이루어진 배열을 `sort()` 해봅시다.

```javascript
const arr = [1, 30, 4, 21, 100000];
arr.sort();

console.log(arr); // [1, 100000, 21, 30, 4]
```

<br>

### [`splice()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)

배열의 기존 요소를 삭제 또는 교체하거나 새 요소를 추가하여 배열의 내용을 변경합니다.

<br>

## 10. 접근자 메소드

<br>

## 11. 순회 메소드

<br>

---

### References

- [JavaScript reference | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference)
- [Standard built-in objects | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects)
- [Arrays | MDN](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps/Arrays)
- [Array | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
- [Array Object | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Obsolete_Pages/Core_JavaScript_1.5_Guide/Predefined_Core_Objects/Array_Object)
- [Grammar and types | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#Array_literals)
- [Array() constructor | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Array)
- [Array.prototype.fill() | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/fill)
- [리터럴과 생성자](https://github.com/yoosoo-won/yoosoo-won.github.io/wiki/%EB%A6%AC%ED%84%B0%EB%9F%B4%EA%B3%BC-%EC%83%9D%EC%84%B1%EC%9E%90)
- [JavaScript typed arrays | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Typed_arrays)