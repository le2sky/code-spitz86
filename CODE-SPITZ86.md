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

Identifier context 특징:-

- 하나의 원본 사용
- 상태 변화를 내부에서 책임짐
- 메세지를 기반으로 로직을 전개

## 2. Polymorphism(다형성)

부모는 자식을 대체할 수 없지만, 자식은 부모를 대체할 수 있다.
확장된 클래스는 확장할 클래스를 대체할 수 있다는 것이 Polymorphism의
핵심이다. 이것을 Polymorphism 안에서 **대체가능성(substitution)**이라고 한다.
**내적일관성(internal identity)**은 어떠한 경우에도 태어났을 떄 원본 클래스를 유지하려는 속성이다. Ploymorphism은 이러한 대체가능성과 내적일관성을 합친 것이다.

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
