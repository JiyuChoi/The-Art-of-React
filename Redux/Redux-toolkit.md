## Redux-toolkit

redux-toolkit은 기존 redux의 여러 문제를 해결한 버전의 모듈

👉🏻 **redux의 문제점**

- 리덕스의 복잡한 스토어 설정
- 리덕스를 유용하게 사용하기 위해서 추가되어야하는 많은 패키지들
- 리덕스 사용을 위해 요구되는 다량의 상용구(boilerplate) 코드들

이러한 문제를 개선하기 위해 redux-toolkit를 사용 → 기존 리덕스의 **복잡도를 낮추고, 사용성을 높여서** 코드를 작성

## Redux Store 생성

```jsx
// store.js

import { configureStore } from "@reduxjs/toolkit";

//redux store
export const store = configureStore({
  reducer: {},
});
```

reducer가 포함된 객체를 configureStore에 전달하면 간단히 redux store가 생성됨

```jsx
import React from "react";
import ReactDOM from "react-dom";
import "./index.css";
import App from "./App";
import reportWebVitals from "./reportWebVitals";

//Provider와 store추가
import { Provider } from "react-redux";
import { store } from "./store";

ReactDOM.render(
  //이곳에서 아래와 같이 Provider에 store를 전달하고 App을 감싼다
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById("root")
);
```

## Slice 생성

기본 redux로 리듀서를 만들 때는 switch를 이용해 액션을 구분하여 상태를 변화시켰고, 디스패치를 호출할 때도 구조화된 액션을 매개변수로 줘야만 했음

redux-toolkit에서는 이러한 복잡한 정의와 사용을 탈피하고자 **createSlice라는 새로운 API를 지원**

```jsx
import { createSlice } from "@reduxjs/toolkit";

const initialState = {
  value: 0,
};

export const counterSlice = createSlice({
	//name은 각 action에 대한 prefix
  name: "counter",
  initialState,
  reducers: {
    increment: (state) => {
      state.value++;
    },
    decrement: (state) => {
      state.value--;
    },
  },
});

//actions
//dispatch할 때 액션을 전달해 상태를 어떻게 변화시킬지를 결정한다
export const { increment, decrement } = counterSlice.actions;
export default counterSlice.reducer;

//
// 이전 reducer 생성 방법

function counterReducer(state = initialState, action) {
  switch (action.type) { 
    case "counter/increment": 
      return { ...state, value: state.value + 1 }; 
    case "counter/decrement":
      return { ...state, value: state.value - 1 }; 
    default: 
      return state; 
  } 
}
```

createSlice에서는 액션이 디스패치되었을 때의 동작을 `switch`가 아닌 `object`형태로 정의 가능

```jsx
// store.js

import { configureStore } from "@reduxjs/toolkit";

//방금 전 counter.js에서 export default한 counterSlice.reducer
import counterReducer from "./redux/counter";

//redux store
export const store = configureStore({
  reducer: {
    //add counterReducer
    counter: counterReducer
  },
});
```

다시 store로 돌아가 reducer에 counter를 추가

## Counter 컴포넌트 생성

```jsx
import React from "react";
import { useDispatch, useSelector } from "react-redux";
import { decrement, increment } from "../redux/counter";

const Counter = () => {
  //useSelector를 통해 redux store에서 특정 값을 관찰할 수 있다
  const count = useSelector((state) => state.counter.value);
  
  //dispatch에 action을 전달하면 해당 동작이 실행된다
  const dispatch = useDispatch();

  //increment(1증가) 동작
  const handleIncrement = () => dispatch(increment());
  //decrement(1감소) 동작
  const handleDecrement = () => dispatch(decrement());
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={handleIncrement}>increment</button>
      <button onClick={handleDecrement}>decrement</button>
    </div>
  );
};

export default Counter;
```

`useSelector`는 reduxt store의 특정 값을 후킹해오는 함수이며 `useDispatch`는 리덕스 스토어에 액션을 전달하기 위한 함수
