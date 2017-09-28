# **Ch5. Spread 연산자**

## 5.1 개요

Spread 연산자는 Iterable Object의 Element를 하나씩 분리하여 전개한다. 전개한 결과를 변수에 할당하거나 호출하는 함수의 parameter 값으로 사용한다.

Spread 연산자는 "..."과 같이 점 세 개를 연속해서 작성하고 이어서 Iterable Object를 작성한다.

### 배열

    let one = [11, 12];
    let two = [21, 22];
    let spreadObj = [51, ...one, 52, ...two];

    console.log(spreadObj);
    console.log(spreadObj.length);

    //결과
    [51, 11, 12, 52, 21, 22]
    6

### 문자열

    let spreadObj = [... "music"];
    console.log(spreadObj);

    //결과
    ["m", "u", "s", "i", "c"]

### 함수 parameter


    const values = [10, 20, 30];
    get(...values);

    function get(one, two, three){
        console.log(one + two + three);
    }

    //결과
    60

## 5.2 rest parameter

호출받는 함수의 파라미터에 function(...rest)와 같이 spread 연산자로 파라미터를 작성한 형태를 rest 파라미터라고 한다. rest의 사전적 의미는 "나머지", "휴식" 이다. 호출 하는 함수의 파라미터에 3개의 파라미터 값을 작성하고 호출받는 함수의 파라미터에 파라미터 이름을 하나만 작성하면 2개의 파라미터 값이 설정되지 않는다. 이때, rest 파라미터를 작성하면 2개의 파라미터 값이 rest 파라미터에 배열 엘리먼트로 설정된다.

    let get = (one) => {
        console.log(one);
    }
    get(...[1,2,3]);

    //결과
    1

get(...[1, 2, 3])이 get(1,2,3) 형태로 전개 되므로 호출받는 함수의 파라미터 one에 1이 설정된다. 호출받는 get()함수가 화살표 함수이므로 호출된 함수에서 보낸 파라미터가 arguments에 설정되지 않는다. 따라서 [1, 2, 3]에서 2와 3을 받지 못하게 된다. 이때, rest 파라미터를 작성하면 2와 3을 받을 수 있다.

    let get = (...rest) => {
        console.log(rest);
        console.log(Array.isArray(rest));
    }

    get(...[1, 2, 3]);

    //결과
    [1, 2, 3]
    true
가독성을 위해 파라미터 명을 rest로 작성한 것으로 rest 파라미터이기 때문에 rest로 작성한 것은 아니다.

    let get = (one, ...rest) => {
        console.log(one);
        console.log(rest);
    }

    get(...[1, 2, 3]);
    //결과
    1
    [2, 3]

파라미터 one에 1이 설정되고 rest에 [2, 3]이 설정된다.

> spread 와 rest의 형태는 갖지만, 기능이 다르므로 구분해야 한다. spread는 호출하는 함수의 파라미터에 사용하며 배열을 엘리먼트 단위로 전개한다. rest는 호출받는 함수의 파라미터에 사용된다. 우선 호출하는 함수의 파라미터 순서에 맞추어 호출받는 파라미터에 값을 설정한다. 설정되지 못하고 남은 파라미터 값을 배열의 엘리먼트로 설정한다.

> spread 파라미터는 배열을 엘리먼트로 분리, 전개한다. rest는 전개된 엘리먼트를 다시 배열에 설정한다.

## 5.3 Array-like

Array는 아니지만 Array처럼 사용할 수 있는 Object 오브젝트를 Array-like라고 한다.

    let values = {0: 'zero', 1: 'one', 2: 'two', 3: 'three'} 

배열은 인덱스를 가지고 있다. 그래서 작성된 순서대로 읽을 수 있으며 인덱스를 참조하여 값을 수정, 삭제할 수 있다. 또한 length 프로퍼티를 가진다.

{key: value} 형태의 Object 오브젝트 특징을 유지하면서 배열의 특징을 가미한 것이 Array-like 이다. {key: value} 형태에서 key에 0부터 1씩 증가하면서 값을 부여한다. 배열의 인덱스 값을 프로퍼티 key 값으로 사용하는 것과 같다. values["0"] 형태로 프로퍼티를 읽을 수 있다. 프로퍼티 값에 배열의 엘리먼트에 해당하는 값을 작성한다. 다만, 프로퍼티 키 값을 일련번호로 사용하는 것이 다르다. 여기에 {length: 3} 과 같이 프로퍼티 수를 저장한다. 이 형태가 Array-like다.

    let values = {0: 'zero', 1: 'one', 2: 'two', length: 3}

    for(var key in values){
        console.log(key, ':', values[key]);
    };

    for(var k = 0; k < values.length; k++){
        console.log(values[k]);
    }

    //결과
    0 : zero
    1 : one
    2 : two
    length : 3
    zero
    one
    two

## 5.4 rest와 arguments 차이

get(1, 2, 3)으로 호출했을 때, 호출받는 함수의 arguments에 1, 2, 3이 설정된다.
arguments도 Array-like이므로 for()문으로 전개할 수 있다.

### Argument 오브젝트

Argument 오브젝트가 Array-like이므로 Array 오브젝트의 메서드를 사용할 수 없다. Object.prototype에 연결된 프로퍼티가 Argument 오브젝트의 __ proto __ 에 설정되므로 Object 오브젝트의 메소드를 사용할 수 있지만, 모든 오브젝트에 Object 오브젝트가 상속되므로 Argument에 한정된 것은 아니다.

화살표 함수에서 arguments를 사용할 수 없다. 함수 안의 코드를 보아야 arguments의 사용 여부와 사용 부분을 알 수 있어 코드 가독성이 떨어진다. 많은 코드를 디버깅하거나 소스 코드를 리팩토링할 때 부담이 된다.

### rest 파라미터

함수 안의 코드를 체크하지 않고 get(one, ...rest) 형태만 보더라도 rest 파라미터  범위를 알 수 있어 가독성이 좋다. rest 파라미터가 배열이므로 Array 오브젝트의 메서드를 사용할 수 있다. 화살표 함수 블록에서 rest 파라미터를 사용할 수 있다. 이와 같은 점을 고려해볼 때 Argumen때 오브젝트보다 rest 파라미터가 효율이 더 높다.