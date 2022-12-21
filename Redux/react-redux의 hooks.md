# react-redux의 hook

## 👉🏻 react-redux의 hook 적용

react-redux 7버전 이후로 react에 대한 redux 관련 hooks API들 제공, 이에 따라 이전의 connect() 작업이 불필요해짐

- 필수) **Provider Wrapping**
    
    ```jsx
    const store = createStore (rootReducer);
    
    ReactDOM.render (
      <Provider store={store}> 
        <App /> 
      </Provider> ,
      document.getElementById('root')
    )
    ```
    
    기존 react-redux와 마찬가지로 최상위를 Provider로 랩핑, 그 **내부의 컴포넌트들은 hooks를 통해 스토어에 접근**할 수 있음
</br>


## 👉🏻 useSelector

state를 조회하기 위함, store에 저장된 **state를 참조하는 hooks**

- mapStateToProps 역할과 유사
- connector 함수를 이용하지 않고 리덕스의 state를 조회할 수 있음


```jsx
import React from 'react'
import { useSelector } from 'react-redux'

export const CounterComponent = () => {
  const counter = useSelector(state => state.counter)
  return <div>{counter}</div>
}
```

```jsx
// Bad (user, isLoggedIn 둘 중 하나라도 바뀌면 재랜더링 됨)
const {user, isLoggedIn} = useSelector((state) => state.user);

// Good (필요한 state만 가져와 사용)
const isLoggedIn = useSelector((state) => state.user.isLoggedIn);
```

스토어에서 **state가 수정되면 이를 참조하는 모든 컴포넌트가 리렌더** 됨 → 최적화를 위해 필요한 state만 가져와 사용해야함

</br>

## 👉🏻 useDispatch

생성한 action을 발생시키기 위함, **dispatch 함수를 실행하는 hooks** 

- mapDispatchToProps 역할과 유사
- 만들어둔 액션 생성 함수를 import 함


```jsx
import React from 'react'
import { useDispatch } from 'react-redux'
import { openLoginModalScreen } from "../reducers/global";

export const CounterComponent = ({ value }) => {
  const dispatch = useDispatch()

  return (
    <div>
      <span>{value}</span>
      <button onClick={() => dispatch(openLoginModalScreen)}>
        Increment counter
      </button>
    </div>
  )
}
```

dispatch 함수이므로, 내부 인자는 action 객체를 받음 → **action 객체나 생성함수를 별도로 만들어주면 유용**

- 규모가 작은 프로젝트는 **각 Reducer 파일에 action 함수**를 포함, 큰 프로젝트는 **Action 디렉토리에 별도로 action 함수들만 모은 파일**로 관리
</br>

## 👉🏻 useSelector, useDispatch 활용 예시

```jsx
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { increment, decrement } from './Actions/Action';

function App() {
  const counter = useSelector(state => state.counter.count)
  const isLogged = useSelector(state => state.logged.isLogged)
  const dispatch = useDispatch();

  return (
    <div className="App">
      <h1>Counter: {counter}</h1>
      {isLogged && <h3>Valuable Information I shouldn't see.</h3>}

      <button onClick={() => dispatch(increment(1))}>+</button>
      <button onClick={() => dispatch(decrement(1))}>-</button>
    </div>
  );
}

export default App;
```
