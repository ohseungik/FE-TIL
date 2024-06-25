# 무한 캐러셀 구현

<br>

## 이전 / 다음 버튼 클릭시

1. 버튼클릭시 이미지를 감싸고 있는 컨테이너 태그를 vw 100% 마이너스 (transform : translatex -100vw%) 으로 움직여주어야함

<br>

2. 가장 첫번째와 마지막 img(배열에서의 첫번째 요소와 마지막요소) 각각 앞뒤에 마지막 요소와 첫번째 요소를 넣어준다.

- 첫번째 이미지가 자연스럽게 마지막으로 넘어가는 것처럼 보이기위해 마지막 이미지를 첫번째 이미지 앞에 만들어 놓는다.
- 마찬가지로 마지막 이미지에서 그다음 첫번째로 이동하는것처럼 보이기위해 첫번째 이미지를 마지막 이미지 뒤에 만들어 놓는다.

<br>

3. 만약 첫번째 케러셀 이미지로 갔을때 가장 마지막 이미지 요소를 이미 앞에 넣어 놨기때문에 버튼을 클릭시 자연스럽게 마지막 이미지 캐러셀로 이동한것처럼 보인다.

<br>

4. 마지막 이미지 캐러셀에서 transform의 시간을 0으로 초기화하여 이동하는 모션을 보여주지 않게끔 만들어준다.

- 첫번째에서 이전 버튼을 클릭시 이미 만들어 놓은 마지막 이미지로 자연스럽게 transition duration이 0.5초라면 setTimout으로 0.5초 이후에 바로 원래 마지막 이미지 페이지로 duration이 0초로 초기화 시켜 바로 넘기게 한다.

<br>

## 코드 구현방법

<br>

1. 이미지가 있는 배열을 map메소드를 통해 li태그 안에 img를 넣어 맵핑 시켜준다.

<br>

2. 그전에 가장 첫번째 배열의 요소(img)를 map 메소드 이전코드에 넣어 li와 img태그를 만들어준다.

<br>

3. 가장 마지막 배열의 요소(img)도 마찬가지로 map메소드 코드 이후에 li와 img태그를 만들어 넣어준다.

<br>

4. 만약 이전버튼을 클릭시 cur가 1 일때 기존에 첫번째 이미지의 앞에 만들어놓은 마지막 이미지로 duration이 0.5초로 이동시킨후 **transform 시간을 0으로 초기화 한뒤 원래 마지막페이지로 바로 이동시켜준다.**

<br>

4-2 다음페이지도 마찬가지로 cur이 배열의 길이인 5이였을때 기존에 만들어놓은 첫번째 이미지로 duration이 0.5초로 이동시킨후

**transform 시간을 0으로 초기화 한뒤 원래 첫번째 이미지로 바로 이동시켜준다.**

<br>

```jsx
import { useEffect } from "react";
import { useRef } from "react";
import {
  setCarousel,
  setCarouselMax,
  setCarouselMin,
} from "../../../modules/main";
import { useDispatch, useSelector } from "react-redux";

const Carousel = () => {
  const dispatch = useDispatch();
  const carouselNumber = useSelector((state) => state.main);

  const containerRef = useRef(null);
  useEffect(() => {
    containerRef.current.style.transform = `translateX(-${carouselNumber}00%)`;
  }, [carouselNumber]);

  const imgArr = [
    "https://img-cf.kurly.com/shop/data/main/1/pc_img_1610827181.jpg",
    "https://img-cf.kurly.com/shop/data/main/1/pc_img_1612094440.jpg",
    "https://img-cf.kurly.com/shop/data/main/1/pc_img_1611795651.jpg",
    "https://img-cf.kurly.com/shop/data/main/1/pc_img_1596164134.jpg",
    "https://img-cf.kurly.com/shop/data/main/1/pc_img_1612149449.jpg",
  ];

  return (
    <div className="relative">
      <button
        onClick={() => {
          if (carouselNumber === 1) {
            setTimeout(() => {
              containerRef.current.style.transition = "";
              containerRef.current.style.transitionDuration = "";
              containerRef.current.style.transitionProperty = "";
              containerRef.current.style.transitionTimingFunction = "";
              setCarouselnumMax();
            }, 500);
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
        }}
        className="absolute z-50 w-13 h-13 bg-pre-button left-91 top-159 focus:outline-none"
      />
      <div ref={containerRef} className="relative">
        <ul className="absolute w-700 ">
          <li className="w-screen float-left">
            <img className="h-370 " alt="img" src={imgArr[imgArr.length - 1]} />
          </li>
          {imgArr.map((img, i) => (
            <li key={i} className="w-screen float-left">
              <img alt={img} key={i} className="h-370 " src={img} />
            </li>
          ))}
          <li className="w-screen float-left">
            <img alt="img" src={imgArr[0]} className="h-370 " />
          </li>
        </ul>
      </div>
      <button
        onClick={() => {
          if (carouselNumber === imgArr.length) {
            setTimeout(() => {
              containerRef.current.style.transition = "";
              containerRef.current.style.transitionDuration = "";
              containerRef.current.style.transitionProperty = "";
              containerRef.current.style.transitionTimingFunction = "";
              setCarouselnumMin();
            }, 500);
            containerRef.current.style.transitionDuration = "0.5s";
            containerRef.current.style.transitionProperty = "all";
            containerRef.current.style.transitionTimingFunction = "ease-in-out";
            setCarouselnum(+1);
          } else {
            containerRef.current.style.transitionDuration = "0.5s";
            containerRef.current.style.transitionProperty = "all";
            containerRef.current.style.transitionTimingFunction = "ease-in-out";
            setCarouselnum(+1);
          }
        }}
        className="absolute right-0 w-13 h-13 bg-next-button right-91 top-159 focus:outline-none"
      />
    </div>
  );

  function setCarouselnum(num) {
    dispatch(setCarousel(num));
  }

  function setCarouselnumMin() {
    dispatch(setCarouselMin());
  }

  function setCarouselnumMax() {
    dispatch(setCarouselMax());
  }
};

export default Carousel;
```

참고

- [https://im-developer.tistory.com/97](https://im-developer.tistory.com/97)