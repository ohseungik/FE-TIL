# Observer Pattern 옵저버 패턴

<br>

옵서버 패턴(observer pattern)은 **객체의 상태 변화를 관찰**하는 관찰자들, 

즉 옵저버들의 목록을 객체에 등록하여 상태 변화가 있을 때마다 메서드 등을 통해 객체가 직접 목록의 각 옵저버에게 통지하도록 하는 디자인 패턴이다.

<br>

![옵저버 패턴](../Images/Observer%20Pattern/Observer%20Pattern-1.png)

<br>

Subject이 상태가 변경하는 객체라고 했을때,

Observer를 연결하여 Subject의 변경사항이 있을때 그때마다

Observer에게 알려주는 것을 옵저버 패턴이라고 한다.

<br>

복잡한 코드에서 의도치 않은 객체의 변경을 추적하는 것은 어려운 일이므로

객체의 변경을 추적하려면 옵저버 패턴을 통해 객체를 참조하는 대상들에게 이를 알릴 수 있다.

<br>

자바스크립트에서는

`addEventListenr` 로 이벤트 핸들러를 등록해 두면,

이벤트가 발생했을때 알려준다. 

<br>

참고 : 위키피디아[[https://ko.wikipedia.org/wiki/옵서버_패턴](https://ko.wikipedia.org/wiki/%EC%98%B5%EC%84%9C%EB%B2%84_%ED%8C%A8%ED%84%B4)]

참고 : 제로초[[https://www.zerocho.com/category/JavaScript/post/5800b4831dfb250015c38db5](https://www.zerocho.com/category/JavaScript/post/5800b4831dfb250015c38db5)]