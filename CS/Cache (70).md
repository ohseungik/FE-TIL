# 메모리 캐시, 디스크 캐시

<br>

캐시에는 두가지 종류가 있다.

서버에서 사용하는 캐시 → CDN

**클라이언트에서 사용하는 캐시 → 브라우저 캐시(HTTP 캐시, 웹 캐시 라고불린다.)**

<br>

HTTP 캐시는 2가지 종류가 있다.

1. 메모리 캐시
2. 디스크 캐시

<br>

## 디스크 캐시

일반적으로 캐시는 HDD(하드 디스크)에 저장이된다.

하지만 **HDD에 접근해서 캐시를 가져오기에는 너무 오랜시간이 걸린다.**

<br>

그래서 디스크 캐시, 즉 **RAM에 저장해서 훨씬 빠르게 접근해 가져올 수 있다.**

<br>

디스크 캐시에는 20MB 정도의 용량을 저장할 수 있다.

<br>

## 메모리 캐시

RAM에 접근하는 것조차 CPU 입장에는 오래걸린다.

캐시 메모리 위치는

- CPU 내부에
- CPU에 매우 가까이에도 위치해 있다.

<br>

그래서 **CPU와 매우 가까이에 위치해 있는 캐시 메모리에 저장해 더 빠르게 접근해서 가져 올 수 있다.**

![메모리 캐시, 디스크 캐시](../Images/Cache/Cache-1.png)

이미지 출처 : [https://leechangyo.github.io/cs/2020/05/19/14.Cache-Memory/](https://leechangyo.github.io/cs/2020/05/19/14.Cache-Memory/)

메모리 캐시는 L1, L2, L3 이렇게 나누어 위치에 따른 기능이 다르다. 더 자세한 정보는 [이곳에서](https://leechangyo.github.io/cs/2020/05/19/14.Cache-Memory/) 공부해도 좋을 것같다.

<br>

메모리 캐시는 매우 작기 때문에 그만큼 저장할 수 있는 사이즈도 작다.

8kb ~ 8MB 정도의 데이터를 저장할 수 있을 정도이다.

<br>

## How? 디스크, 메모리 캐시에 어떻게 접근할까?

![메모리 캐시, 디스크 캐시](../Images/Cache/Cache-2.png)

<br>

### 메모리 캐시

메모리 캐시는 **웹을 파싱하는 스레드와 같은 스레드에서 요청하는 작업을 한다.**

해당 작업을 **요청하는 곳은 WebCore이다.**

따라서 단순히 함수 호출을 통해 진행된다.

요청과 동시에 객체에 접근해 찾기 때문에 사실상 접근시간이 0에 가깝다.

<br>

### 디스크 캐시

디스크 캐시 위치를 접근하는 스레드는 웹을 파싱하는 스레드(WebCore)와 **다른 스레드(ChromeNetwork)이다.(앞으로 ChromeNetwork라고 하겠다.)**

<br>

디스크 캐시에 접근하는 과정을 알아보자.

1. ChromeNetwork에 디스크 캐시에 접근해달라고 요청하기전, 리소스 우선순위에 따라 시간이 더 걸릴 수 도 있다.
   - 예를들어) HTML, CSS, JS 순으로 우선순위가 높기 때문에, 이미지 같은경우는 잠시 기다리게 된다.
     → 우선순위가 있는 이유는 DOM과 CSS을 먼저 만들어 Render Tree를 만든뒤 **초기 화면을 빠르게 보여주기 위해서이다.**
2. WebCore에서 **ChromeNetwork에 디스크 캐시에 접근해달라고 요청을 한다.**
3. ChromeNetwork에 요청을 하게되면 **디스크 캐시에 접근해서 캐시를 가져오게된다.**

<br>

이 과정에서 시간이 걸린다고 한다.

<br>

참고

- [https://d2.naver.com/helloworld/1059747](https://d2.naver.com/helloworld/1059747)
- [https://mygumi.tistory.com/275](https://mygumi.tistory.com/275)
- [https://aws.amazon.com/ko/caching/](https://aws.amazon.com/ko/caching/)
- [https://leechangyo.github.io/cs/2020/05/19/14.Cache-Memory/](https://leechangyo.github.io/cs/2020/05/19/14.Cache-Memory/)