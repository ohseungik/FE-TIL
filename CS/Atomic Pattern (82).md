- [아토믹 디자인 패턴](#아토믹-디자인-패턴)
  - [아토믹 디자인 패턴이란?](#아토믹-디자인-패턴이란)
  - [어떻게 사용할까?](#어떻게-사용할까)
    - [비지니스 로직 상태관리](#비지니스-로직-상태관리)
  - [아토믹 디자인 패턴의 장점](#아토믹-디자인-패턴의-장점)
  - [아토믹 디자인 패턴의 단점](#아토믹-디자인-패턴의-단점)
    - [1. 컴포넌트간의 의존성이 커지게된다.](#1-컴포넌트간의-의존성이-커지게된다)
    - [2. 디자이너와 같이 설계해야한다.](#2-디자이너와-같이-설계해야한다)
    - [3. 단위 구분하기](#3-단위-구분하기)
  - [정리](#정리)

<br>

# 아토믹 디자인 패턴

## 아토믹 디자인 패턴이란?

아토믹 디자인 패턴은 **어플리케이션를 구조화하는 방법이다.**

컴포넌트를 **작은 단위로 구분해서 분리**하는 개념이다.

**일관된 UI, 재상용성, 유지보수**를 향상시켜준다.

<br>

React 공식 문서에서도 설계 패턴에 대한 대표적인 사례가 없다.

대부분 **많이 쓰이는 패턴은 프레젠테이셔널 컴포넌트와 컨테이너 컴포넌트를 분리**하는 방법이다.

- **데이터를** 다루는 컨테이너 컴포넌트
- **UI를** 다루는 프레젠테이셔널 컴포넌트

<br>

**프레젠테이셔널, 컨테이너 컴포넌트 패턴**은 하나의 페이지를 전체로 작성하기 때문에

- **중복되는 컴포넌트 코드**를 작성하게 되고
- **재사용성**이 떨어지고
  - 재사용을 하기위해서는 **다른 컴포넌트의 컨테이너 폴더에서 불러와야** 한다는 점에서 유지보수가 어렵고 **해당 파일이 어디있는지 찾아야 한다는 단점**이 있다.
- **CSS의 상속**을 고려해서 만들어야한다는 점이 있다.

<br>

물론 이러한 패턴도 큰 장점이 있지만, **아토믹 디자인 패턴을 통해 단점을 보완**할 수 있다는 점이 있다.

당연히 아토믹도 단점이 있으므로 프로젝트의 환경에 따라 **어떤 패턴을 도입할지를 잘 비교해서 적용**해야한다.

<br>

## 어떻게 사용할까?

**하나의 페이지**가 가장 큰 단위이다.

**가장 작은 단위인 HTML Element**들이 모여 페이지를 구성한것을 개념으로 잡고있다.

<br>

따라서 크게 **5가지의 단위들이 있으며, 각각의 하위 단위들이 모여 상위 단위를 구성하는 방식이다.**

![아토믹 디자인 패턴](../Images/Atomic%20Pattern/Atomic%20Pattern-1.png)

Atoms(원자) → Molecules(분자) → Organisms(유기체) → Templates(템플릿) → Pages(페이지)

<br>

**원자는** 버튼, 인풋 등 과 같은 **UI의 최소 단위, 가장 작은 단위로써 더 이상 분해할 수 없는 단위이다.**

**분자**는 2개 이상의 원자로 구성되있으며 하나의 단위로 함께 동작하는 UI 단위이다. 예를 들어 검색 폼 등

**유기체**는 여러 분자들이 모여 **여러 기능들이 합쳐져있는 단위이다.** 예를 들어 풋터, 캐러셀, 헤더..등등

**템플릿**은 유기체인 컴포넌트들을 배치한뒤 **하나의 페이지의 레이아웃을 형성하는 단위이다.** 실제 스타일이 들어가지 않은 골격을 의미한다.

**페이지**는 레이아웃만 형성되어 있는 템플릿에 여러 **CSS 스타일을 적용한 완성된 형태이다.**

![아토믹 디자인 패턴](../Images/Atomic%20Pattern/Atomic%20Pattern-2.gif)

<br>

**원자, 분자, 유기체**는 다른 컴포넌트에서 **재사용이 가능하므로** 같이 묶어서 관리한다.

**템플릿, 페이지**는 **재사용을 사용하지 않으므로** 둘을 묶어서 관리한다.

![아토믹 디자인 패턴](../Images/Atomic%20Pattern/Atomic%20Pattern-3.png)

<br>

### 비지니스 로직 상태관리

만약 컴포넌트의 상태를 관리하는 경우에는 **원자 컴포넌트보다 그 상위인 분자, 유기체에서 상태 관리하는 것이 좋다.**

원자 컴포넌트를 재사용하는 상위 컴포넌트들이 모두 동일한 상태 사용하는것을 기대하기 힘들기 때문이다.

<br>

예를들어) A 버튼은 단순히 텍스트를 가지고 있고, B 버튼은 클릭에따라 색깔이 변경된다.

A, B 두 컴포넌트는 똑같은 디자인을 가지고 있다.

하지만 **B 버튼은 상태를 가지고 있어야하며, 그에따라 색깔이 변경되어야한다.**

<br>

따라서 원자 컴포넌트에서 상태를 가지는 것이아니라

B 버튼은 상태를 가지고 있는 분자 컴포넌트로 만들어야한다.

<br>

따라서 다음과 같이

- atoms/A.Button.jsx
- molecules/B.Buttons.jsx

로 만들어야한다.

<br>

비슷한 컴포넌트이지만, 다른 부류의 컴포넌트를 가지고 있게된다는 단점이있다.

<br>

## 아토믹 디자인 패턴의 장점

팀안에서 공통된 인식을 가진 상태에서 확장성이 높은 코드를 기술하는데 도움이 된다.

<br>

## 아토믹 디자인 패턴의 단점

### 1. 컴포넌트간의 의존성이 커지게된다.

예를 들어) 헤더의 중 일부를 변경해서 앵커 태그로 동작하게 수정해달라고 요청이 왔다.

1. **헤더에 props를 전달**해 특정 Element가 조건에 따라 앵커 태그로 동작하게 하는 방법
2. 앵커 태그가 포함된 **헤더 컴포넌트 전체를 다시 만드는 방법**

<br>

1번 방법을 사용한다면, 간단하게 수정 하면된다.

하지만 만약 많은 요청사항이 들어오게 된다면, 헤더에 그 많은 props들을 전달하는것은 **유지보수가 어렵고 모든 코드를 파악하기 힘들다.**

<br>

따라서 많은 요청사항이 온다면, 2번 방법을 사용해야한다.

2번 방법은 **새로운 헤더 컴포넌트를 만들어서 교체**해주어야한다.

만약 이러한 **요청이 자주온다면**, **새로운 헤더 컴포넌트들을 많이 만들게** 되고 각각의 컴포넌트들이 **어떤 동작을 해야하는지 명확하게 구분하기 어렵다.**

<br>

또한 **컴포넌트 간의 의존성이 상하로 발생해 하위 컴포넌트에서 에러가 발생한다면, 모든 상위 컴포넌트 또한 문제가 발생한다.**

<br>

### 2. 디자이너와 같이 설계해야한다.

아토믹 디자인 컴포넌트 단위 디자인 및 개발을 실현하기 위해서는 **디자이너와 같이 설계해야한다.**

디자인 단계에서부터 컴포넌트 단위의 체계화된 설계가 이뤄져야한다.

되도록이면 **같은 디자인의 형태로 컴포넌트들을 구성**해야하고 **재사용을 고려해서 디자인을 해야한다.**

<br>

아토믹 컴포넌트 분류에 대해서 설명해야하기 때문에 오히려 **의사소통 비용이 증가한다.**

<br>

### 3. 단위 구분하기

특정 컴포넌트를 **원자, 분자, 유기체인지 각각 팀원들과 논의하는 과정을 거쳐야한다.**

또는 작성한 개발자의 마음대로 유연하게 생각해서 **디렉토리를 크게 구분하지 않고 작성할 수도 있다.**

<br>

이러한 문제를 **합의를 이루는 과정이 소비된다는 비용이 있다.**

<br>

## 정리

**React 어플리케이션은 날이 갈수록 발전해나가고 복잡해져가고 있다.** 이러한 과정에서 효율적인 설계 패턴을 찾기가 어렵다.

아토믹 디자인 패턴을 사용하게 된다면, 재사용, 일관성, 유지보수를 향상 **시켜줄때도 있다.**

<br>

하지만 프로젝트 규모가 커진다면, 그 만큼 시간적 비용이 더 많이 들고 복잡해진다는 단점이 있다.

아토믹 디자인 패턴을 공부해가는 과정에서 여러 개발자분들의 대다수의 의견은 **장점도 있지만 복잡도가 증가하는 여러 단점들도 존재해서 도입하기 어렵다는 의견이였다.**

<br>

아토믹을 적용하려고 한다면, 아토믹 디자인 패턴의 정석대로 하지말고 **복잡도를 높이지 않고 재사용성이 가능하게 팀원들과 조율하는것이 필수적이라고 할수 있다.**

<br>

**완벽하게 재사용성을 위한 아토믹 디자인 패턴은 큰 부작용을 가져올 수 있다.**

<br>

참고

- [https://ui.toast.com/weekly-pick/ko_20200213](https://ui.toast.com/weekly-pick/ko_20200213)
- [https://brunch.co.kr/@skykamja24/580](https://brunch.co.kr/@skykamja24/580)
- [https://sumini.dev/guide/009-dont-use-atomic-design/](https://sumini.dev/guide/009-dont-use-atomic-design/)
- [https://jbee.io/web/components-should-be-flexible/](https://jbee.io/web/components-should-be-flexible/)
- [https://www.youtube.com/watch?v=33yj-Q5v8mQ](https://www.youtube.com/watch?v=33yj-Q5v8mQ)