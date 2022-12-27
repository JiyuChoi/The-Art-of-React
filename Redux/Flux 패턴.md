# Flux 패턴
</br>

## MVC 

### 👉🏻 MVC 패턴

데이터를 다루는 로직(Controller), 데이터(Model), 사용자 인터페이스(View)를 나누어 어플리케이션을 구현하는 하나의 개발 모델

![MVC 패턴](https://user-images.githubusercontent.com/75539452/209616691-58a5d097-e1a7-4514-bb4b-30f3c93d0ed8.png)

Controller는 Model의 데이터를 조회하거나 업데이트하는 역할을 하며, Model의 변화는 View에 반영됨.
또한, 사용자가 View를 통해 데이터를 입력하면, Model에 영향을 주면서 데이터를 관리

사용자 인터페이스와 비지니스 로직을 처리하는 구간(**로직, 데이터, 뷰**)을 나누어 관리하기 때문에 효율적

![대규모 MVC](https://user-images.githubusercontent.com/75539452/209616742-9e27a5c2-4508-4294-8bdc-ebb430a30c04.png)

그러나 Model과 View가 서로 **양방향으로 의존**되어 있는 관계 때문에 Facebook 같은 대규모 웹 앱에서의 MVC 구조는 굉장히 복잡해짐 → 이에 따라 **예측불가능한 상황**들 발생 (MVC 패턴의 한계)

</br>

## Flux 패턴의 등장 배경

Facebook은 기존 MVC 패턴을 가진 대규모 애플리케이션에서 구조가 복잡해지고 예측성이 떨어지는 문제점에 직면함

이즈음 Facebook에 로그인하면 화면 위 메시지 아이콘에 새로운 알림이 있다고 표시는 되지만, 클릭하면 새로운 메시지가 없는 버그가 발생

![**Facebook 알림 버그**](https://user-images.githubusercontent.com/75539452/209616757-8aa37891-6234-4f98-a33a-00d2ab6de9cd.png)

이 문제를 해결하기 위해서는 조금 더 예측가능한 형태로 코드를 구조화하는 것이 필요

→ **React**와 **Flux**를 사용해 대규모 애플리케이션에서 **데이터 흐름을 일관성 있게 관리함**으로써 프로그램의 `예측가능성(Predictability)`을 높임

</br>

## **Flux 패턴**

그래서 Facebook은 **단방향 데이터 흐름**을 가지는 Flux 패턴에 대해 발표

해당 패턴을 사용하면 개발 흐름이 단방향으로 흐르기 때문에 **파악하기 쉽고, 코드의 흐름이 예측가능** 해짐

![Flux 패턴](https://user-images.githubusercontent.com/75539452/209616845-a266e2cd-4a2f-4f9f-a497-d485d120b300.png)

### 👉🏻 Action

- 데이터를 변경하는 행위로서 **Dispatcher에게 전달되는 객체**
- Action creator 메서드는 새로 발생한 Action의 `타입(type)`과 `새로운 데이터(payload)`를 묶어 Dispatcher에게 전달

```jsx
{
  type: "EVENT_TYPE",
  data: {
    "name": "Huurray"
  }
}
```

### 👉🏻 Dispatcher

- 모든 데이터 흐름을 관리하는 **중앙허브**
- Store들이 등록해놓은 Action 타입마다의 맞춤 Callback 함수들이 있음
- Action을 감지하면 Store들이 타입에 맞는 Store의 Callback 함수를 실행하도록 함
- Store의 데이터를 조작하는 것은 오직 Dispatcher를 통해서만 가능

### 👉🏻 Store

- **상태 저장소**로서 **데이터**와 **데이터를 변경할 수 있는 메서드**를 가지고 있음
- 어떤 타입의 Action이 발생했는지에 따라 그에 맞는 데이터 변경을 수행하는 콜백 함수를 Dispatcher에 등록
- Dispatcher에서 콜백 함수를 실행하여 상태가 변경되면 View에게 데이터가 변경되었음을 알림

### 👉🏻 View

- View는 **리액트 컴포넌트**로 생각
- Flux에서의 View는 MVC의 뷰와는 달리, 화면을 보여주는것 외에도 **Controller**의 성격도 가지고 있음
(최상위 View는 Store에서 데이터를 가져와 이를 자식 View 로 내려보내주는 역할을 함)
- 새로운 데이터를 받은 View는 화면을 리렌더링 함
- 사용자가 View에 어떤한 조작을 하면 그에 해당하는 Action을 생성합니다.

</br>

각 요소들은 **단방향 흐름에서 순서대로 역할이 수행**되고 또 다시 새로운 데이터 변경이 있으면 처음부터 이 흐름이 다시 시작됨
→ 이렇게 함으로써 **예외없이 데이터를 처리**할 수 있게 됨

따라서 Facebook은 Flux 패턴을 고안하고 View의 역할로 **React**를 이용했으며, 이로인해 **더 예측 가능하고 확장성이 높은 앱**을 만들 수 있게 되었음
