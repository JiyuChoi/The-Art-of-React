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
