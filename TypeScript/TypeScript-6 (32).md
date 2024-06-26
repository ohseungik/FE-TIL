- [TypeScript(6) - 함수끼리 대입, Infer, satisfies](#typescript6---함수끼리-대입-infer-satisfies)
  - [함수끼리 대입](#함수끼리-대입)
    - [반환값의 경우](#반환값의-경우)
    - [매개변수의 경우](#매개변수의-경우)
    - [메서드의 경우](#메서드의-경우)
  - [Infer](#infer)
  - [satisfies](#satisfies)

# TypeScript(6) - 함수끼리 대입, Infer, satisfies

<br>

## 함수끼리 대입

함수끼리 대입하는 방법은 공변성과, 반공변성, 이변성에 대해 알아야한다.

공변성 : A → B 일때 `T<A>` → `T<B>`인 경우

반공변성 : A → B 일때 `T<B>` → `T<A>` 인경우

이번성 : A → B 일때 `T<A>` → `T<B>`도 되고 `T<B>` → `T<A>` 되는 경우

<br>

여기서 A와 B는 반환값을 의미하고 `T<a>, T<b>`는 함수라고 생각하면 된다.

예시를 보면 더 쉽다.

<br>

### 반환값의 경우

```tsx
function a(x: string): string {}
type B = (x: string) => string | number;

let b: B = a; // a를 b에 할당. b가 a보다 더 크므로 할당 가능
```

함수 a`를 타입 `b`에 할당할 수 있는 이유는 `b`의 반환 타입이 `a` 의 반환 타입보다 더 포괄적인 넓은 범위를 가지고 있기 때문이다.

여기서 a → b 이고 `T<a>` → `T<b>` 를 할당할 수 있으므로 공변성을 갖는다고 할 수 있다.

**항상 반환값은 공변성을 가지고 있다고 보면된다. a → b(b가 크면 항상 a를 대입할 수 있다.)**

<br>

### 매개변수의 경우

strict 옵션이 true 인경우 반공변성을 가진다.

```tsx
function a(x: string | number): number {}
type B = (x: string) => number;

let b: B = a; // 매개변수 a가 b의 매개변수보다 큰데도 대입할 수 있다. 반공변성
```

여기서 반환값은 같고 매개변수의 타입만 다르다.

a함수의 매개변수는 string | number이고 b함수의 매개변수는 string이다.

즉, b → a 인 상황이지만, 여기서 매개변수는 반공변성을 가진다고 했으므로 `T<a> → T<b>` 이 된다.

반대는 에러가 발생한다.(`T<b> → T<a>`는 에러가 발생함)

<br>

**결론은 매개변수는 반공변성을 가진다.**

하지만 주의할 점으로, 여기서 strict 옵션을 false로 해제하면 `T<b>` → `T<a>`는 에러가 발생하지 않는다.

즉, strict 옵션이 아닐때는 이변성을 가진다.

<br>

### 메서드의 경우

객체의 메서드인 경우는 메서드를 타이핑 하는 방식에 따라 다르다.

```tsx
interface SayMethod {
  say(a: string | number): string;
}

interface SayFunction {
  say: (a: string | number) => string;
}

interface SayCall {
  say: {
    (a: string | number): string;
  };
}

const sayFunc = (a: string) => "hello";
const MyAddingMethod: SayMethod = {
  say: sayFunc, // 이변성
};
const MyAddingFunction: SayFunction = {
  say: sayFunc, // 에러 -> 반공변성
};
const MyAddingCall: SayCall = {
  say: sayFunc, // 에러 -> 반공변성
};
```

기본적으로 sayFunc 함수의 타입은 `(a: string) ⇒ string` 이라서 매개변수 반공변성으로 인해 `(a: string | number) ⇒ string` 에 대입할 수 없다.

```tsx
const obj = {
  a: B,
};
```

**보통 여기서 a 메서드를 B타입에 대입한다라고 한다.**

**B타입을 a 메서드에 대입한다가 아니다.**

<br>

## Infer

'infer'는 TypeScript에서 사용되는 키워드 중 하나로, 제네릭 타입에서 타입 매개변수를 추론할 때 사용된다.

주로 함수나 클래스와 같은 제네릭 타입을 작성할 때 유용하게 활용된다.

'infer' 키워드는 주로 조건부 타입(conditional types) 내에서 사용된다.

조건부 타입은 입력된 타입에 따라 타입을 변환하거나 결정하는 TypeScript의 강력한 기능 중 하나이다.

<br>

구체적으로 말하면, `infer E`는 **입력된 타입**에서 **추론된 타입**을 `E`라는 이름으로 가져오는 것을 의미한다.

이는 조건부 타입의 **입력된 타입에서 추론된 타입**을 나타내는 데 사용된다.

여기서 **‘입력된 타입에서 추론된 타입’** 이부분이 중요하다.

<br>

```tsx
type El<T> = T extends (infer E)[] ? E : never;
type Str = El<string[]>; // string
type NumOrBool = El<(number | boolean)[]>; // string | boolean
```

여기서 `El<string[]>` 의 `string[]` 을 추론해서 string만 빼낸것이 E이다.

<br>

주의사항! 컨디셔널 타입에서 타입 변수는 참 부분에서만 쓸 수 있다.

다음과 같이 하면 에러가 발생한다.

```tsx
type El<T> = T extends (infer E)[] ? never : E; // 에러
```

<br>

더 많은 부분을 스스로 추론할 수 있다.

다음은 매개변수, 생성자 매개변수, 반환값, 인스턴스 타입을 추론하는 타입이다.

```tsx
type MyParameters<T> = T extends (...args: infer P) => any ? P : never;
type Result = MyParameters<(a: number, b: string) => string>;
// type Result = [a: number, b: string]

type MyConstructorParameter<T> = T extends abstract new (
  ...args: infer P
) => any
  ? P
  : never;
type Result2 = MyConstructorParameter<new (a: number, b: string) => {}>;
// type Result2 = [a: number, b: string]

type MyReturnType<T> = T extends (...args: any) => infer R ? R : never;
type Result3 = MyReturnType<(a: number, b: string) => string>;
// type Result3 = string

type MyInstanceType<T> = T extends new (...args: any) => infer R ? R : any;
type Result4 = MyInstanceType<new (a: number, b: string) => {}>;
// type Result4 = {}
```

<br>

서로 다른 타입 변수를 여러개 동시에 사용할 수 있다.

```tsx
type MyPAndR<T> = T extends (...args: infer P) => infer R ? [P, R] : never;
type PR = MyPAndR<(a: string, b: number) => string>;
// type PR = [[a: string, b: number], string]
```

<br>

반대로 같은 타입 변수를 여러곳에 사용할 수 있다.

```tsx
type Union<T> = T extends { a: infer U; b: infer U } ? U : never;
type Result1 = Union<{ a: 1 | 2; b: 2 | 3 }>;
// type Result1 = 1 | 2 | 3
```

첫번째 a infer U 는 1 | 2, 두번째 b infer U는 2 | 3 따라서 1 | 2 | 3 이된다.

같은 이름의 타입 변수(여기선 infer U)는 서로 유니온이 된다.

<br>

## satisfies

타입 추론을 그대로 활용하면서 추가로 타입 검사를 하고 싶을때 사용한다.

아래의 코드는 일부로 universe의 속성 키중 sirius 대신 siriius로 오타를 냈다.

그리고 굳이 타이핑을 하지않고 추론을 통해 타입을 정했다.

```tsx
const universe = {
  sun: "star",
  sriius: "star",
  earth: { type: "planet", parent: "sun" },
};
```

이럴때 타입 추론이 아니라 타입을 정할때 sriius의 오타를 잡아낼 수 있는 방법이 있다.

<br>

```tsx
const universe: {
  [key in "sun" | "sirius" | "earth"]:
    | { type: string; parent: string }
    | string;
} = {
  sun: "star",
  sriius: "star",
  earth: { type: "planet", parent: "sun" },
};
```

위와 같이 인덱스 시그니처를 사용해 타이핑 해서 siriius의 오타를 잡을 수 있다.

<br>

다만 속성 값을 사용할 때가 문제이다. earth의 타입이 객체라는 것을 제대로 잡아내지 못한다.

```tsx
universe.earth.type; // <--- 에러가 발생한다.
```

속성 값의 타입을 객체와 문자열의 유니언으로 표기했기 때문에 earth가 문자열 일 수 도 있다고 생각하는것이다.

<br>

이럴때 처음처럼 타입을 추론하는 이점을 사용하고 sriius에서 오타가 났음을 알리는 방법이 있다.

satisfies 연산자를 사용하면 된다.

```tsx
const universe = {
  sun: "star",
  sriius: "star",
  earth: { type: "planet", parent: "sun" },
} satisfies {
  [key in "sun" | "sirius" | "earth"]:
    | { type: string; parent: string }
    | string;
};
```

이러면 universe 타입은 타입 추론된것을 그대로 사용하면서, 각각 속성들은 satisfies에 적은 타입으로 **다시 검사 한다.**

여기서 sririus 오타가 발견된다.

또한 `universe.earth.type` 도 에러없이 쓸 수 있다.

<br>

참고

- 타입스크립트 교과서를 정리한 글입니다.
- https://www.typescriptlang.org/docs/handbook/2/narrowing.html#using-type-predicates
- [https://velog.io/@devshk447/TIL-typescript-type-guard와-type-predicates](https://velog.io/@devshk447/TIL-typescript-type-guard%EC%99%80-type-predicates)