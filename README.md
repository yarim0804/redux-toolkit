# redux-toolkit
Redux-toolkit 세미나


## Redux란?
- 상태관리 라이브러리 중 하나

## Redux의 기본 용어
### Store
스토어는 컴포넌트의 상태를 관리하는 저장소다. 하나의 프로젝트는 하나의 스토어만 가질 수 있다.

### Action
스토어의 상태를 변경하기 위해서는, 액션을 생성해야한다. 액션은 객체이며, 반드시 type을 가져야 한다. 액션 객체는 액션생성함수에 의해서 만들어진다.

### Reducer
리듀서는 현재 상태와 액션 객체를 받아 새로운 상태를 리턴하는 함수다.

### Dispatch
디스패치는 스토어의 내장 함수 중 하나이며, 액션 객체를 넘겨줘 상태를 업데이트 시켜주는 역할을 한다.

### Subscribe
스토어의 내장 함수 중 하나로, 리듀서가 호출될 때 서브스크라이브된 함수 및 객체를 호출한다.


## Redux가 필요한 이유

### Redux가 없다면?
  Props -> Props -> Props ... Props drilling 야기

  <img src="https://velog.velcdn.com/images/sweet_pumpkin/post/4b69bb9b-26ce-4c95-b04c-b79dd3bf1b07/image.gif"/>

  ### Redux가 있다면?

  <img src="https://velog.velcdn.com/images/sweet_pumpkin/post/ed1d68d9-3b90-4c95-8b54-ff3e5c6e7d47/image.gif"/>


## 불변성 조건 + spread 개념( 얕은, 깊은 복사)

## createSlice, configureStore

## RTK Query
