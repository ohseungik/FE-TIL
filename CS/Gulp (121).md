# Gulp
<p align="center">
  <a href="http://gulpjs.com">
    <img height="257" width="114" src="https://github.com/wonism/TIL/blob/master/front-end/gulp/img/gulp.jpg">
  </a>
</p>

## 걸프란?
- Gulp 는 간편한 Java Script 빌드 도구이다.
  - 빌드 과정, 테스트, 배포 등을 자동화하여 명령어로 간단하게 실행할 수 있다.
  - npm package 를 사용할 수 있다.
  - 일반적으로 전처리된 CSS, Java Script 컴파일 및 연결과 최소화 등을 도와준다.
  - 모듈을 require 로 받아서 여러 태스크를 구성한다.
- Grunt 와 하는 일은 비슷하지만,
  - Grunt 는 JSON 을 기반으로 선언적인 설정을 통해 태스크를 관리한다.
  - Gulp 는 코드로 태스크를 관리한다.
- 빌드 시스템은 단순히 task runners 라고 불리며, 자동화된 태스크들의 모음이다.

## 설치 및 사용방법
- OS X 기준
```sh
# npm 설치
$ brew install npm

# gulp 설치
$ npm i gulp
# 혹은
$ yarn add gulp

# gulpfile 생성
$ touch gulpfile.js

# default 수행
$ gulp

# TASK_A 수행
$ gulp TASK_A

# 변화 감지하기
$ gulp -w
```