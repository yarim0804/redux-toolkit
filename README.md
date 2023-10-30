# redux-toolkit 세미나 2023-10-31(화)

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
### Action
Action은 어떤걸 하는지에 대한 지시가 있는 객체이다. 액션의 type 속성이 반드시 필요하며 이를 바탕으로 리듀서에 작동하게 하는 것이 가능하다.

### Action Creator
Action Creator는 액션을 언제든지 사용할 수 있도록 만들어 주는 함수이다.

### Dispatch
디스패치는 스토어의 내장 함수 중 하나이며, 만들어진 액션 객체를 리듀서에 보내 넘겨줘 상태를 업데이트 시켜주는 역할을 한다.

### Store
스토어는 컴포넌트의 상태(state)를 관리하는 저장소다. 하나의 프로젝트(앱)는 기본적으로 하나의 스토어만 가질 수 있다.

### Reducer
리듀서는 현재 상태와 액션 객체를 받아 새로운 상태를 리턴하는 함수다. 
단, 리듀서는 순수함수여야 하고, 리듀서 안에서 동적인 처리나 변화는 있어서는 안 된다. (불변성 법칙)


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
- 기본적인 Redux 작업을 간단하게 만드는 API를 제공 (패키지 의존성을 줄여줌)
- 기존 redux에서는 불변성 조건에 의해 항상 spread를 사용하여 다른 state들은 변경되지 않게 유지하여야 하는 코드를 작성해야만 했음


## createSlice, configureStore
### createSlice
  - createAction, createReducer 함수가 내부적으로 내장되어 있다.
  - 리듀서를 작성할 수 있게 도와주는 함수이다.
  - 반드시 'name', 'initialState', 'reducers' 의 속성은 필수이다.
  - 'name'에 지정된 prefix값을 가지고 createSlice가 내부적으로 유니크한 이름을 만들어 준다.
  - 'initialState'는 reducer에서 사용할 값을 지정한다.
  - 'reducers'는 사용하는 함수를 모두 정의할 수 있으며 매개변수로는 'state', 'action'이 존재한다.

### configureStore
  - 기존 redux에서는 createStore를 사용하기 위해 항상 combined 된 store를 rootReducer 값으로 보냈어야 한다.
  - 그 외에도 thunk, applyMiddleware, reduxDevTools 모두 수행하여야 했다.

#### 기존 Reducer
```javascript
let initialState = {
    productList : [],
    selectedItem : null
};

function productReducer( state = initialState, action ){
  let {type, payload} = action;
  switch (type){ // 매번 switch case 만들어야 함 (action에서 호출하는 이름과 동일해야 함)
    case "GET_PRODUCT_SUCCESS": // 유니크한 이름으로 지정해야 함
      return {...state, productList: payload.data };
    case "GET_SINGLE_PRODUCT_SUCCESS":
      return {...state, selectedItem: payload.data };
    default:
      return {...state};
  }
}

export default productReducer;
```

#### RTK를 사용한 Reducer
```javascript
import {createSlice} from "@reduxjs/toolkit";

let initialState = {
  productList : [],
  selectedItem : null
};

const productSlice = createSlice({
  name : "product",
  initialState,
  reducers : { // actions에 해당 함수들이 지정됨
    getAllProducts(state, action){
      state.productList = action.payload.data; // 객체가 아니게 됐으므로 = 로 지정
    },
    getSingleProduct(state, action){
      state.selectedItem = action.payload.data; // dispatch에서 지정한 객체가 payload 안으로 들어가는걸 알 수 있음
    }
  }
});

export const productActions = productSlice.actions; // dispatch 할 때 사용할 수 있게 export 처리

export default productSlice.reducer; //store에 전달해야하는 것은 reducer
```


#### 기존 Store
```javascript
import { createSTore, applyMiddleware } from "redux";
import { composeWithDevTools } from "redux-devtools-extension";
import thunk from "redux-thunk";
import rootRecuder from "./reducers";

let store = createStore(
  rootReducer,
  composeWithDevTools(applyMiddleware(thunk))
);

// combinereducer, thunk, applyMiddleware, reduxDevTools 사용했어야만 했음 

export default store;
```

#### RTK를 사용한 Store
```javascript
import {configureStore} from "@reduxjs/toolkit";

const store = configureStore({ // combineReducer 안에 있던 객체를 여기다 지정
  reducer:{
    auth : authenticateReducer,
    product : productReducer,
  }
})

export default store;
```

#### dispatch 방법
```javascript

//기존
dispatch({type: "GET_PRODUCT_SUCCESS", payload: {data});

//RTK
import {productActions} from "../reducers/productReducer";

dispatch(productActions.getAllProducts({data})); // 매개변수 값은 알아서 payload 아래로 들어감

```

## RTK Query
서버에서 받아온 데이터 상태를 관리할 수 있는 라이브러리로 RTK의 연장선이라 볼 수 있음.

  - 서버와의 통신 과정에서 로딩 상태, 에러 여부 등을 관련 컴포넌트 내부에서 직접 상태를 작성하여 관리하지 않아도 되며, props drilling을 할 필요도 거의 없어진다.
  - 자동 데이터 캐싱을 통해 서버의 부담도 줄일 수 있다.
  - 일정 시간이 지나거나, 데이터 변동이 생겼을 때(유저가 게시글을 작성하는 등) 자동으로 캐시된 데이터를 제거하고 다시 받아와 유저에게 (적당히) 최신의 데이터를 보여줄 수도 있다.

