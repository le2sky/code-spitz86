## 1. MVC

model-view-controller가 있다. mvc에는 두가지 방법이 있다.

**1. 고전적인 mvc 모델** : 모델을 컨트롤러가 가져와서 뷰가 소비할 수 있는 형태로 데이터를 가공해서 뷰한테 전달한다.

<p align="center">
    <img src= "resource/mvc1.PNG">
</p>

- 컨트롤러가 모델을 알고 있다.
- 컨트롤러가 뷰도 알고 있다.
- 뷰는 유저의 인터랙션을 받아들여서 모델을 갱신한다.
- 뷰는 자기가 실제로 어떠한 모델을 갱신해야 하는지 알기 때문에 모델에 대한 의존성을 가진다.

뷰가 모델을 알고 있다는 것이 mvc의 가장 큰 문제이다. 모델은 비즈니스 도메인에 관련된 것이고, 자주 변경된다.
뷰는 그림이 안이쁘거나 등 UI에 대해 변화가 많다. 이 두가지는 서로 변하는 이유가 다른데 둘이 의존성을 가지고 있다.
또한 모델의 변화가 간접적으로 data의 변화에 영향을 준다.

사용 예) 서버에서 주로 사용된다. 왜냐하면 view와 model의 의존성이 없기 때문이다.

**2. 제왕적 컨트롤러 mvc 모델**: 뷰와 모델의 의존성이 없고, 뷰는 컨트롤러를 알게된다.

<p align="center">
    <img src= "resource/mvc2.PNG">
</p>

- 실질적으로 뷰가 모델을 의존하는 것은 없어진다.
- 뷰의 변화와 모델의 변화를 모두 컨트롤러가 흡수해야 한다.
- 컨트롤러 유지보수가 힘들어진다.
- 컨트롤러가 서비스 로직을 불러오거나, DI를 사용하거나, 디스패쳐를 부르는 식으로 겨우 사용한다.

## 2. MVP

model-view-presenter는 4세대 랭귀지가 출현했을 때, 나왔다. 비쥬얼 베이직 등이 해당된다.
뷰는 getter와 setter를 보이는 인터페이스를 가지고 있다. 프레젠터의 입장에서는 특정한 뷰의 getter와 setter를 호출한다.

<p align="center">
    <img src= "resource/mvp.PNG">
</p>

- 뷰는 모델을 모른다. (뷰에 대한 모델의 의존성 완전 제거)
- getter와 setter에 대응하는 기능을 프레젠터에 작성해서 대응
- 뷰 하나를 만들려고 해도 뷰의 모든 기능의 getter,setter를 맵핑해야 한다.
- 뷰가 그림을 그리는 재량권을 잃어버린다.
- 프레젠터가 getter, setter를 통해서 통제한다.
- 뷰 컴포넌트가 무거워진다.

## 3. MVVM

Model-View-ViewModel 모델로 ms에서 만들었다.

<p align="center">
    <img src= "resource/mvvm.PNG">
</p>

- Binder가 있어야 성립된다.
- ViewModel은 순수한 뷰이다. (그림을 그리는 뷰가 아닌 인메모리 객체로서, 순수한 객체로서 뷰)
- 뷰의 변화가 생기면 바인더를 통해서 뷰모델을 갱신
- 뷰모델의 변화가 생기면 바인더와 뷰가 자동으로 이것을 감지해서 뷰를 갱신
- 바인더는 뷰와 뷰모델의 의존성을 자신으로 만듦으로서 뷰와 뷰모델의 의존성을 제거한다.
- 뷰모델은 뷰를 모른다. (이것이 목적)
- 바인더가 뷰와 뷰모델의 변화를 관찰한다. (양방향 바인딩이나 단방향 바인딩은 선택적)

## 4. TypeCheck

```js
const type = (target, type) => {
  if (typeof type == "string") {
    if (typeof target != type) throw `invaild type ${target} : ${type}`;
  } else if (!(target instanceof type))
    throw `invaild type ${target} : ${type}`;
  return target;
};

type(12, "number");
type("abc", "string");
type([1, 2, 3], Array);
type(new Set(), Set);
type(document.body, HTMLElement);

const test = (arr, _ = type(arr, Array)) => {
  console.log(err);
};

const test2 = (
  a,
  b,
  c,
  _0 = type(a, "string"),
  _1 = type(b, "number"),
  _2 = type(c, "boolean")
) => {
  console.log(a, b, c);
};
```

## 5. View hook & bind

```html
<section id="target" data-viewmodel="wrapper">
  <h2 data-viewmodel="title"></h2>
  <section data-viewmodel="contents"></section>
</section>
```

<p align="center">
    <img src= "resource/viewhook.PNG">
</p>

## 6. Role Design
