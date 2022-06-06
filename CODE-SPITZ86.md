## 1. Value Context vs Identifier Context

함수형 프로그래밍인지, 객체지향 프로그래밍인지 프로시저 프로그래밍인지
를 구분하는 잣대보단 값이라는 컨텍스트를 사용할지 식별자 기반 컨텍스트를 사용할지가 중요하다.

Value Context는 메모리 주소와 관계없이 그 값이 같다면 같다고 생각하는 관점이다. 예를 들어 변수 a,b가 모두 3이라는 값을 가지고 있다면 다른 메모리 상에 존재한다고 해도 같다고 판단한다.
어떤 변수에 저장되어 있는지 보단, 어떤 값을 가지고 있는지가 중요하다.

반대로 Identifier Context는 같은 값이라도 메모리 주소가 다르다면 다른 값이라고 생각하는 관점이다.

객체지향 프로그래밍은 value context를 배제한다. 특별한 상황을 제외하고는
둘 중 하나의 컨텍스트를 사용하는 것을 권장한다.

```js
const a = {
  a: 3,
  b: 5,
};
const b = {
  a: 3,
  b: 5,
};

console.log(a === b); // Identifier context
console.log(JSON.stringify(a) === JSON.stringify(b)); // Value context
```

Value context 특징:

- 끝 없는 복사본을 생성한다.
- 상태 변화에 안전하다.
- 연산을 기반으로 로직을 전개한다.

Identifier context 특징:

- 하나의 원본 사용
- 상태 변화를 내부에서 책임짐
- 메세지를 기반으로 로직을 전개

## 2. Polymorphism(다형성)

부모는 자식을 대체할 수 없지만, 자식은 부모를 대체할 수 있다.
확장된 클래스는 확장할 클래스를 대체할 수 있다는 것이 Polymorphism의
핵심이다. 이것을 Polymorphism 안에서 **대체가능성(substitution)** 이라고 한다.
**내적일관성(internal identity)** 은 어떠한 경우에도 태어났을 떄 원본 클래스를 유지하려는 속성이다. Ploymorphism은 이러한 대체가능성과 내적일관성을 합친 것이다.

```js
const Worker = class {
  run() {
    conosle.log("working");
  }
  print() {
    this.run(); //내적일관성
  }
};

const HardWorker = class extends Worker {
  run() {
    console.log("hardWorking");
  }
};

const worker = new HardWorker();
console.log(worker instanceof Worker); //대체가능성
worker.print();
```

**Polymorphism의 정체** :

확장된 객체는 원본으로 대체 가능하며 생성 시점의 타입이 내부에 일관성 있게 참조된다.

## 3. Polymorphism of Prototype

자바스크립트의 내적일관성을 보장하는 방법

<p align="center">
  <img src="./resource/polymorphismofprototype.png">
</p>

## 4. Object essentials

```js
const EssentialObject = class {
  #name = "";
  #screen = null;
  constructor(name) {
    this.#name = name;
  }
  camouflage() {
    this.#screen = (Math.random() * 10).toString(16).replace(".", "");
  }
  get name() {
    return this.#screen || this.#name;
  }
};
```

- **hide state(은닉) - Maintenance of State**: 객체의 모든 속성은 private으로 선언(데이터 은닉)
  - : 객체지향은 메모리의 참조로 움직여야 하는데 속성이 공유되는 순간 속성을 값으로 취득해서 쓰기 때문에 값 컨텍스트가 프로그램 전체에 만연하게 되고,
    결국에는 값 컨텍스트로 인해 객체지향이 무너지게 된다.
  - : 내부의 상태를 감춘다.
  - : 자신의 상태에 대한 관리의 책임이 있어야 한다.
- **encapsulation(캡슐화) - Encapsulation of Functionality**: 내부에서 무슨 일이 일어나는지 노출하면 안 된다(객체의 메소드에서 일어나는 일은 외부에서 알면 안 된다)
  - : ATM은 내부적으로 굉장히 복잡하게 작동하지만, 사용자는 그러한 일들에 대해 알 필요도 없고 알아서도 안 된다.
  - : 외부에서 내부의 일을 모르게 한다.
  - : 예를 들어 setAge 라는 method는 캡슐화에 위배될 수 있다. setChild setAdult 같은 method로 캡슐화할 수 있다.

이 두가지 본질을 보장하는 객체는 변화에 대한 해당 객체의 격리구간을 만들 수 있다.(Isolation of change)
함수형 프로그래밍이나 객체지향 프로그래밍은 변화에 대한 격리에 초점을 맞춘다.

## 5. SOLID 원칙

- SRP(Single Responsibility) : 단일책임 원칙

  - 수정해야 하는 원인이 하나 밖에 없다. (코드를 고쳐야할 이유는 하나이다.)
    SRP를 지키지 못하면 일어나는 산탄총수술(Shotgun surgery)가 있다.

  - **산탄총수술** : 객체 하나를 건들이면 수많은 사이드이펙트가 생긴다.

- OCP(Open Closed) : 개방폐쇄 원칙

  - 확장에는 열려 있고(extends 나 implements), 요구사항이 변경되었을 때 객체의 수정은 닫혀있다.

- LSP(Liskob Substitusion) : Liskov 치환 원칙, 업캐스팅 안전

  - 부모 쪽으로 업캐스팅하는 것이 안전함을 보장한다.
  - 추상층의 정의가 너무 구체적이면 구상층의 구현에서 모순이 발생한다.

아래의 사진과 같이 추상층이 구체적이면 구상층의 구현에서 모순이 발생한다. 아래와 같은 경우에
Liskov 치환 원칙을 위배한다.

<p align="center">
  <img src="./resource/liskovsubstitusion.png">
</p>

아래와 같이 다리로 이동한다를 제외하면 모두가 행복하면서도 업캐스팅에 안전하다. 하지만 몇몇 동물은 다리로 이동한다는 기능을 잃어버렸다.

<p align="center">
  <img src="./resource/liskovsubstitusion3.png">
</p>

아래와 같이 두가지 추상층으로 분리한 이후 적용하면 모두가 행복하면서도 업캐스팅에 안전하게 작성되었다.

<p align="center">
  <img src="./resource/liskovsubstitusion2.png">
</p>

- ISP(Interface Segregation) : 인터페이스분리 원칙
  - 위의 사례와 같이 LSP를 위반하는 사례를 ISP를 통해 해결할 수 있었다.
  - 인터페이스를 분리해서 문제를 해결한다.

아래와 같이 여러 모듈이 하나의 객체의 여러 기능에 의존한다. 역할에 따라서 본다면 서로 다른 모듈은
공통된 메서드를 사용하지 않는다. 즉, 각 모듈마다 하나의 객체를 바라보는 관점이 다르다.
상태의 본질은 다른 사람에게 위임할 수 없기 때문에 의존되는 객체는 메서드를 모두 가지고 있어야한다.
이렇게 모든 메서드가 구분이 안되면 모든 모듈은 의존되는 객체를 인식하게 된다. 나중에 객체가 변경된다면 이 부분에서 문제가 생길 수 있다.

<p align="center">
  <img src="./resource/ISP1.png">
</p>

인터페이스를 분리하지 않는 방법 중 하나는 소유(위임)를 사용해서 이 문제를 해결하는 것이다. 상위 객체의 분신을 여러개를 만든다. 분신들은 상위 객체의 자식이기 때문에 상위 객체의 상태를 공유할 수 있다.

<p align="center">
  <img src="./resource/ISP2.png">
</p>

상위 객체를 만들 때, 여러 인터페이스로 분리하고
인터페이스에 맞게 오버라이드해서 구현한다.

<p align="center">
  <img src="./resource/ISP3.png">
</p>

- DIP(Dependency inversion) : 의존성 역전의 법칙, 다운캐스팅 금지

  - 의존성은 부모쪽으로 흘러야한다.
  - 다른 solid 원칙이 다 지켜져야 다운캐스팅을 막을 수 있다.
  - 고차원의 모듈(더 자식 쪽)은 저차원의 모듈에 의존하면 안된다. 이 두 모듈 모두 추상화된 것에 의존해야 한다.
  - 추상화 된 것은 구체적인 것에 의존하면 안된다. 구체적인 것이 추상화된 것에 의존해야한다.

- **DI(dependency injection)** : IoC(제어역전)의 일부분이다.
- **DRY** : Don't Repeat Yourself : 중복방지
- **Hollywood Principle** : 의존성 부패방지, 시간날 때 나한테 연락줘(요청), 시간날때 내가 연락할게
- **Law of demeter** : 최소지식, 어떤 객체에 대한 지식을 알때 최소한으로 알아야한다 그렇지 않으면 의존성 부패가 일어난다.
  - classA.methodA의 최대지식 한계
    - classA의 필드 객체
    - methodA가 생성한 객체
    - methodA의 인자로 넘어온 객체
      => 지키지 않으면 열차전복(train wreck)
      => 원래 알아야하는 대상보다 더 많은 객체를 알아버리기 때문이다.
