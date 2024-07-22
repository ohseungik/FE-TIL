# 여러번 발생하는 이벤트 호출 방지

<br>

만약 버튼을 여러번 눌렀을때 가장 처음으로 발생하는 이벤트만 실행시켜준다.

<br>

그이후 발생하는 이벤트들은 무시한다.

<br>

## 코드구현

<br>

- 만약 클릭이 첫번째 인경우에는 이벤트가 실행하게 한다.
- 만약 클릭이 0.5초이내에 실행된다면 이벤트가 실행하지 못하게한다.

<br>

1. `let onAinamate = false`로 이벤트가 여러번 호출 되었는지 안되었는지를 확인해주기 위해 변수 선언을 해준다.
2. 첫번째 초기값을 fasle로 해주어서 처음 조건문에서 false인 경우에는 조건에 걸리지 않아 올바르게 이벤트 코드가 실행된다.
3. 만약 true 값이 되었을 경우에는 return이 되어 뒤의 코드가 실행되지 않는다.
4. false로 조건문에 들어가지 않았을경우 true값으로 바꾸어준다.
5. 그리고 원하는 시간만큼 이벤트가 계속 발생하지 않게 setTimeout으로 0.5초뒤에 false값으로 바꾸어주어 0.5초 이내에서는 계속 trued으로 유지시켜주고 0.5초 뒤에는 false값으로 만들어준다.

<br>

```jsx
let onAnimate = false;

<button
  onClick={() => {
    if (onAnimate) return;
    onAnimate = true;
    if (carouselNumber === 1) {
      containerRef.current.style.transitionDuration = "0.5s";
      containerRef.current.style.transitionProperty = "all";
      containerRef.current.style.transitionTimingFunction = "ease-in-out";
      setCarouselnum(-1);
    } else {
      containerRef.current.style.transitionDuration = "0.5s";
      containerRef.current.style.transitionProperty = "all";
      containerRef.current.style.transitionTimingFunction = "ease-in-out";
      setCarouselnum(-1);
    }
    setTimeout(() => {
      onAnimate = false;
    }, 500);
  }}
  className="absolute z-50 w-p-52 h-p-52 bg-pre-button left-p-91 top-p-159 focus:outline-none"
/>;
```