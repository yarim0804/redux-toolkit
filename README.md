# redux-toolkit
Redux-toolkit 세미나

<br/>
<br/>

## Redux란?
- 상태관리 라이브러리 중 하나
- "전역" 상태를 포함하는 단일 스토어
- 앱에 어떤 일이 일어날 때 스토어에 일반 객체 액션을 디스패치하는 것
- 액션을 살펴보고 불변성을 유지한 채 업데이트된 상태를 반환하는 순수 리듀서 함수

<br/>
<br/>

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

<br><img src="https://velog.velcdn.com/images/sweet_pumpkin/post/53df187d-3402-4ec1-be28-1fd13dcbba57/image.gif"/>
<br>
1. UI가 처음 렌더링될 때, UI 컴포넌트는 리덕스 스토어의 상태에 접근하여 해당 상태를 렌더링한다.
2. 이후 UI에서 상태가 변경되면, 앱은 디스패치를 실행해 액션을 일으킨다.
3. 새로운 액션을 받은 스토어는 리듀서를 실행하고 리듀서를 통해 나온 값을 새로운 상태로 저장한다.
4. 서브스크라이브된 UI은 상태 업데이트로 변경된 데이터를 새롭게 렌더링한다.

<br/>
<br/>

## Redux가 필요한 이유

### - Redux가 없다면?
  Props -> Props -> Props ... Props drilling 야기

  <img src="https://velog.velcdn.com/images/sweet_pumpkin/post/4b69bb9b-26ce-4c95-b04c-b79dd3bf1b07/image.gif"/>

  ### - Redux가 있다면?

  <img src="https://velog.velcdn.com/images/sweet_pumpkin/post/ed1d68d9-3b90-4c95-8b54-ff3e5c6e7d47/image.gif"/>

<br/>
<br/>

## Redux-toolkit 을 사용하는 이유
- 보일러플레이트 코드 제거
- 기본적인 Redux 작업을 간단하게 만드는 API를 제공


## 불변성 조건 + spread 개념( 얕은, 깊은 복사)
JavaScript는 기본적으로 가변 언어이며, 불변 업데트를 작성하려면 수동으로 객체 복사(spread)와 배열 업데이트가 필요합니다

## createSlice, configureStore
configureStore는 한 번의 호출로 Redux 스토어를 설정하며, 리듀서를 결합하고 thunk 미들웨어를 추가하고, Redux DevTools 통합을 하는 등의 작업을 수행합니다. 또한, 이름이 있는 옵션 매개변수를 사용하기 때문에 createStore보다 구성이 쉽습니다.
createSlice는 Immer 라이브러리를 사용하는 리듀서를 작성할 수 있게 해줍니다. 이를 통해 state.value = 123과 같은 "변형 (mutating)" JS 문법을 spreads 없이도 불변성을 유지하며 업데이트할 수 있습니다. 또한, 각 리듀서에 대한 액션 생성자 함수를 자동으로 생성하고, 리듀서 이름에 기반하여 내부적으로 액션 타입 문자열을 생성합니다. 마지막으로, TypeScript와 잘 호환됩니다.

## RTK Query
