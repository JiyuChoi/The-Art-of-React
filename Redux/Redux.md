## Redux를 사용하는 이유

### 1. Props 귀찮을 때
👉🏻 Redux 미사용
- 상위 component의 state(변수)를 하위 component에서 사용하려면 props를 통해 전송해야함 → component가 많아진다면? 귀찮아.. redux 사용

👉🏻 Redux 사용
- state를 저장하는 store.js 파일 생성 → 모든 component들이 직접 꺼내서 사용 가능 (useSelector 사용)

   즉, 모든 component가 props 없이 state 직접 꺼내서 사용 가능
<br>

### 2. 상태 관리가 용이
- state 관리가 용이하다는 뜻

👉🏻 Redux 미사용
- 컴포넌트들이 특정 state 변경하기 시작함 → 갑자기 버그가 생김
- 버그 추적 시, 모든 component를 다 탐색해야함 → component가 많아진다면? 언제 그걸 다 보고 있어! 귀찮아.. redux 사용

👉🏻 Redux 사용
- store.js 파일에 state 변경되는 모든 방법을 다 넣는다 (Reducer에 If문 사용하여 작성, state++, state— 등등) → API를 만드는 느낌
- component에서 직접 state를 변경하는 것이 아닌 store.js 파일에 수정 요청만 전달 (useDispatch)
- 버그 발생 시, 추적이 쉬움 (store.js에서만 state 관리를 함)
</br>

## Redux 기본 용어

- **Store**
    
    스토어는 컴포넌트의 상태를 관리하는 저장소, 하나의 프로젝트는 하나의 스토어만 가질 수 있음
    
- **Action**
    
    스토어의 상태를 변경하기 위해서는 액션을 생성해야함. 액션은 객체이며, 반드시 type을 가짐.
    액션 객체는 액션 생성함수에 의해서 생성됨
    
- **Reducer**
    
    리듀서는 현재 상태와 액션 객체를 받아 새로운 상태를 리턴하는 함수
    
- **Dispatch**
    
    디스패치는 스토어의 내장 함수 중 하나이며, 액션 객체를 넘겨줘 상태를 업데이트 시켜주는 역할을 함
    
- **Subscribe**
    
    스토어의 내장 함수 중 하나로, 리듀서가 호출될 때 서브스크라이브된 함수 및 객체를 호출
    
<img src="https://user-images.githubusercontent.com/75539452/209518097-3137385a-e786-4b5c-b7e7-8915901694eb.gif" width="600" height="400"/>


1. UI가 처음 렌더링될 때, UI 컴포넌트는 리덕스 스토어의 상태에 접근하여 해당 상태를 렌더링
2. 이후 UI에서 상태가 변경되면, 앱은 디스패치를 실행해 액션 발생
3. 새로운 액션을 받은 스토어는 리듀서를 실행하고 리듀서를 통해 나온 값을 새로운 상태로 저장
4. 서브스크라이브된 UI은 상태 업데이트로 변경된 데이터를 새롭게 렌더링
