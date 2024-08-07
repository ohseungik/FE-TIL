# 디바운스 / 스로틀링

<br>

scroll, resize, input, mousemove, mouseover 같은 이벤트는 짧은 시간 간격으로 연속해서 발생한다. 이러한 이벤트에 바인딩한 이벤트 핸들러는 과도하게 호출되어 성능에 문제를 일으킬 수 있다.

<br>

디바운스와 스로틀은 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 **과도한 이벤트 핸들러의 호출을 방지하는 프로그래밍 기법이다.**

<br>

## 디바운스

디바운스(debounce)는 짧은 시간 간격으로 이벤트가 연속해서 발생하면 이벤트 핸들러를 호출하지 않다가 일정 시간이 경과한 이후에 **이벤트 핸들러가 한 번만 호출되도록 한다.**

<br>

index.html

```jsx
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>debounce</title>
    <style>
      .container {
        width: 300px;
        height: 300px;
        background-color: rebeccapurple;
        overflow: scroll;
      }

      .content {
        width: 300px;
        height: 1000vh;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <div class="content"></div>
    </div>
    <p>스크롤된 횟수</p>
    <span class="count">0</span>
    <script src="./debounce.js"></script>
  </body>
</html>
```

<br>

debounce.js

```jsx
let $container = document.querySelector(".container");
let $count = document.querySelector(".count");

function debounce(callback, delay) {
  let timeid;

  return () => {
    // 이전의 setTimeout의 호출이 사라지게 된다.
    if (timeid) clearTimeout(timeid);

    timeid = setTimeout(callback, delay);
  };
}

let count = 0;
$container.addEventListener(
  "scroll",
  debounce(() => {
    $count.textContent = ++count;
  }, 300)
);
```

delay 시간보다 짧은 간격으로 이벤트가 발생하면 이전 타이머를 취소하고 새로운 타이머를 재설정한다.

<br>

따라서 delay보다 짧은 이벤트가 발생하면 지속적으로 발생하지 않다가 마지막에 delay보다 시간이 지나면 **마지막 한번만 발생하게 된다.**

<br>

## 스로틀링

스로틀은 짧은 시간 간격으로 연속해서 발생하는 이벤트를 delay한 시간 만큼만 발생하게 한다.

<br>

따라서 연속적으로 발생하지 않고 타이머를 설정한 시간만큼만 이벤트가 발생한다.

<br>

index.html

```jsx
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>throttle</title>
  </head>
  <style>
    .container {
      width: 300px;
      height: 300px;
      overflow: scroll;
    }

    .content {
      width: 300px;
      height: 1000vh;
      background-color: tomato;
    }
  </style>

  <body>
    <div class="container">
      <div class="content"></div>
    </div>

    <span>스로틀링된 횟수</span>
    <span class="count">0</span>
    <script src="./throttle.js"></script>
  </body>
</html>
```

<br>

throttle.js

```jsx
let $container = document.querySelector(".container");
let $count = document.querySelector(".count");
function throttle(callback, delay) {
  let timeid;

  return () => {
    // 이전에 이벤트가 있었다면 현재 이벤트를 취소해준다.
    if (timeid) return;
    timeid = setTimeout(() => {
      callback();

      // 이전에 이벤트가 있었다면 timeid를 초기화 해준다.
      // 초기화 해주지 않으면 항상 timeid는 값을 가진다.
      // 따라서 조건문에의해 절대 실행되지 않는다.
      timeid = null;
    }, delay);
  };
}

let count = 0;

$container.addEventListener(
  "scroll",
  throttle(() => {
    $count.textContent = count++;
  }, 300)
);
```

<br>

delay 이전에 이벤트가 발생하면 해당 이벤트를 아예 취소(`return`)시켜준다.

<br>

이후 이전 타이머가 delay 시간뒤에 실행되면 timeid가 `null` 값을 가져서 초기화 되기때문에 다음 이벤트는 그대로 조건문에 걸리지 않고 실행되어진다.

<br>

## 정리

디바운스

- 마지막실행
  1. 최초 `setTimeout` 내부에서 콜백함수 실행
  2. timeId가 있는 경우(delay 시간 전에 이벤트 발생시)
  3. `clearTimeout`으로 최초 `setTimeout` 취소후 다시 `setTimeout` 내부에서 콜백함수을 다시 실행

<br>

스로틀링

→ 중간중간 실행

1. 최초 `setTimeout` 내부에서 콜백함수 실행
2. timeId가 있는경우(delay 시간 전에 이벤트 발생시)
3. 함수 실행 X (주의! 기존의 `setTimeout`을 취소하는게 아니라 현재 실행하려는 함수 실행X )

<br>

참고

- 모던 자바스크립트 deep dive 책을 참고해서 정리한 글입니다.