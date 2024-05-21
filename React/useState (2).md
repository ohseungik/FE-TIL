## useState란?

useState는 함수형 또는 클래스형 컴포넌트의 상태를 관리하고, 변경할 수 있도록 도와주는 하나의 React Hook이다.
<br>
useState는 React에서 제공하는 다양한 React Hooks 중에 하나로, 함수형 또는 클래스형의 컴포넌트에서 로컬의 데이터 상태를 관리할 수 있게 만들어 주는 기능이다.
<br>
useState는 초기 입력될 상태 값을 인자로 받아서 상태 값과 해당 상태를 업데이트하는 함수를 쌍으로 반환하게 된다.

다음의 코드로 예시를 들어본다.

```javascript
const [value, setValue] = useState(0);
```

해석하면 다음과 같다.
<br>
value는 저장된 값. 즉 useState(0)에서 0의 값을 갖게 된다.
<br>
setValue의 경우 value 변경된 값을 저장할 수 있게 만들어 준다.
<br>

useState은 동적 데이터를 다루는 것, 컴포넌트별 고유한 상태 값, 변경 가능성(실시간 or 주기적 렌더링이 필요한 UI), 상태관리를 통한 컴포넌트 내부에 캡슐화를 활용하여 UI 업데이트 등에 사용된다.
<br> 
클래스 컴포넌트 형태의 경우. 생성자 "this.변수"를 통하여 값의 상태를 관리하고, 함수형 컴포넌트의 경우. useState를 활용하여 관리할 수 있다.

### 1. 함수형 코드

```javascript
import React, { useState } from 'react';

const Counter = () => {
  const [count, setCount] = useState(0);

  const incrementCounter = () => {
    setCount(count + 1);
  };

  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={incrementCounter}>Increment</button>
    </div>
  );
};

export default Counter;
```

### 2. 컴포넌트형 코드

```javascript
import React, { Component } from 'react';

class Counter extends Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  incrementCounter = () => {
    this.setState({ count: this.state.count + 1 });
  }

  render() {
    return (
      <div>
        <h1>Count: {this.state.count}</h1>
        <button onClick={this.incrementCounter}>Increment</button>
      </div>
    );
  }
}

export default Counter;
```

<br>
다음과 같이 useState를 사용 할 수 있다.