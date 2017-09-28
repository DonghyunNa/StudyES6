# **Ch.4 Iteraation**

## 4.1 개요
Iteration의 사전적 의미는 되풀이, 반복이다. 반복 처리를 나타내며 이를 위한 프로토콜을 가진다.
프로토콜이라고 하면 통신 프로토콜이 연상된다. 통신에 있어 프로토콜은 약속된 기준과 방법으로 데이터를 송수신하는 것을 의미한다. 즉, 통신 프로토콜은 통신 규약이다. ES6에서 프로토콜도 맟나가지로 규약이다.

프로그래밍에서 반복 처리는 Array 오브젝트가 떠오른다. Array를 반복하여 처리하기 위해서는 배열이 반복할 수 있는 오브젝트여야 한다. 오브젝트에 반복 처리를 할 수 있는 메서드가 있어야 한다. 이것이 ES6의 Iteration Protocol 이다.

Iteration Protocol은 Iterable Protocol과 Iterator Protocol로 구성된다.

## 4.2 Iterable Protocol

Iterable Protocol은 오브젝트의 반복 처리 규약을 정의한다. built-in String, Array, Map, Set, TypedArray, Argument 오브젝트는 디폴트로 Iterable Protocol을 갖고 있다. 또한 DOM의 NodeList도 갖고 있다.

이와 같은 오브젝트는 자바스크립트 엔진이 랜더링될 때 Iterable Protocol이 설정된다. 사전 처리를 하지 않아도 반복 처리를 할 수 있다. Iterable Protocol이 설정된 오브젝트를 Iterable Object 라고 한다.

자바스크립트는 Iterable Object에 Symbol.iterator가 있어야 한다. 이것이 Iterable Protocol이다. 거꾸로 보면 Symbol.iterator가 있으면 Iterable Object 이다. 자체 오브젝트에는 없지만, 상속받은 prototype chain에 있어도 Iterable Object가 된다. 예를들면, built-in Array Object를 상속받은 Object는 Iterable Object가 된다.

프로토콜이 규약이므로 non Iterable Object에 프로토콜을 지켜 개발자 코드를 추가하면 Iterable Obejct가 된다.
즉, Iterable Object가 아닌 Object에 Symbol.iterator를 개발자 코드로 추가하면 Iterable Protocol을 지키게 되므로 Iterable Object가 된다. Symbol.iterator에 반복 처리할 수 있도록 코드를 작성하면 된다.

이 장에서는 개념을 다루고 구현 방법은 Ch.17에서 다룬다.

    let arrayObj = [];
    let result = arrayObj[Symbol.iterator];
    console.log(result);

    //결과 : function values() { [native code] }

Array 오브젝트에 할당된 arrayObj에서 Symbol.iterator의 존재 여부를 체크하는 코드다. 오브젝트에 프로퍼티 존재 여부를 체크할 때 arrayObj.propertyKey 또는 arrayObj[propertyKey] 형태로 작성하지만, Symbol은 arrayObj.Symbol.iterator 형태로 작성할 수 없고 arrayObj[Symbol.iterator]와 같이 대괄호 []안에 Symbol.iterator를 작성해야 한다.

built-in Array Object에 Symbol.iterator가 설정되어 있어서 위와 같은 결과를 얻었다.

    let objectObj = {};
    let result = objectObj[Symbol.iterator];
    console.log(result);

    //결과 : undefined

Object 오브젝트에 Symbol.iterator가 없으므로 Object 오브젝트는 Iterable Object가 아니다.

# 4.3 Iterator Protocol
Iterator Protocol은 오브젝트의 값을 차례대로 처리할 수 있는 방법을 제공한다. 이 방법은 오브젝트에 있는 next() 메서드로 구현한다. 따라서 오브젝트에 next()메서드가 있으면 Iterator Protocol이 적용된 것이다.

자바스크립트에서 { key: value } 형태의 Object 오브젝트는 작성한 순서대로 열거되는 것을 보장하지 않는다. 이는 Object 오브젝트에 next() 메서드가 없다는 의미이기도 하다. ES6은 Object 오브젝트에 추가한 순서대로 key, value가 열거 되는 Map 오브젝트를 제공한다. ch.20 에서 다룬다.

```지금 까지의 내용을 정리하면 Iterable Object는 반복 가능한 오브젝트를 나타내고 Iterator Protocol은 Iterable Object의 값을 작성한 순서대로 처리하는 규약이다.```

    let arrayObj = [1, 2];
    let iteratorObj = arrayObj[Symbol.iterator]();
    console.log("1: ", typeof iteratorObj);

    console.log("2: ", iteratorObj.next());
    console.log("3: ", iteratorObj.next());
    console.log("4: ", iteratorObj.next());

    //결과 :
    1: object
    2: Object {value: 1, done: false}
    3: Object {value: 2, done: false}
    4: Object {value: undefined, done: true}

[1, 2]로 Array 오브젝트를 생성하여 arrayObj에 할당한다. arrayObj의 Symbol.iterator()를 호출하면 Iterator Object를 생성하여 반환한다.
[Symbol.iterator]()로 반환받은 iteratorObj 타입은 object이다. object라는 것은 오브젝트에 next() 메서드가 포함되어 있다는 것을 의미한다. 따라서 iteratorObj.next()형태로 호출할 수 있다.

[1, 2]로 생성한 Array 오브젝트는 Iterable Object이고, [Symbol.iterator]()를 호출하여 변환받은 오브젝트는 Iterator Object이다.

두번째 콘솔출력의 {value: 1}은 [1, 2]의 1이고, {done: false} 는 이터레이터가 종료되지 않은 것을 의미한다. Iterator Object의 next() 메서드는 이터러블 오브젝트의 값을 읽어 {value: 1, done: false} 형태로 반환한다.

세번째 콘솔출력은 두번째 콘솔출력의 다음 단계이다.

마지막 콘솔출력은 Array의 원소가 두개 이며, 다음으로 읽을 데이터가 없기 때문에 undefined 그리고 이터레이터가 종료되었기 때문에 {done: true}가 출력된다.

>  이와 같이 Iterator를 사용하여 Iterable Object의 값을 작성한 순서대로 하나씩 읽을 수 있다. for()문으로 반복 처리 하는 것과는 차이가 있다. 목적도 다르다.