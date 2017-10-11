# Ch.8 Object

## 8.1 오퍼레이션

ES6에서 Object 오브젝트에 프로퍼티를 작성하고 제어하는 방법이 추가 되었다.

### Object에 같은 key 사용

    var obj = {key: value}
위와 같은 형태에서 key 값이 같은 프로퍼티를 두 개 작성했을 때 자바스크립트 버젼 별로 차이가 있다. ES3에서는 key 값이 같아도 추가되고, ES5의 strict 모드에서는 error가 발생. `ES6에서는 strict 모드에 관계없이 에러가 발생하지 않으며 나중에 작성된 프로퍼티 값으로 대체된다.`

### 변수 이름으로 값 설정

    let one = 1, two = 2;
    let values = {one, two};
    console.log(values);
    //결과
    Object {one: 1, two: 2}

### Object에 function 작성

ES5에서 Object에 함수를 다음 형태로 작성한다.

    let obj = {
        getTotal: function(param) {
            return param + 123;
        }
    };

ES6에서는 다른 방법으로 메소드를 작성한다. getTotal(param){} 형태와 같이 잓겅한다. 사전 설명이 필요하므로 관련된 곳에서 다룬다.

## 8.2 디스크립터

Descriptor는 ES5에서 제시되었으며 ES6에서 이를 바탕으로 여러 기능이 추가되었다. 
    
    Object.defineProperty({}, "book", {
        value : 123,
        enumerable :  true
    });

위 코드에서 "book"이 프로퍼티 이름이다. 뒤에 객체가 프로퍼티 디스크립터인데, 속성이름(enumerable)과 값(value) 으로 구성된다. 자세한 내용은 구글링.

## 8.3 get, set 속성

### get
    var obj = {};
    Object.defineProperty(obj, "book", {
        get: function(){
            return "책";
        }
    });
    console.log(obj.book);
    //결과
    책

엔진이 obj.book 코드를 해석하면 obj 오브젝트에서 book 프로퍼티 작성 여부를 체크한다. 작성되어 있으면 get 속성의 존재 여부를 체크한다. 존재하면 get 속성 값인 함수를 실행한다. 이것이 getter 이다. ES5에서는 이와 같이 get 속성으로 getter를 정의한다.

getter는 obj.book()과 같이 함수를 호출하는 형태로 작성하지 않고, obj.book과 같이 함수 이름만 작성한다. getter가 호출되면 "책"을 반환한다.

### set

    var obj = {};
    Object.defineProperty(obj, "item", {
        set: function(param){
            this.sports = param;
        }
    });
    obj.item = "야구"
    
    console.log(obj.sports)
    //야구

위 get과 set은 ES5의 방식이다.
## 8.4 getter

    let obj = {
        value: 123,
        get getValue(){
            return this.value;
        }
    };
    console.log(obj.getValue);

    //결과
    123

### 8.5 setter

    let obj = {
        set setValue(value){
            this.value = value;
        }
    }
    obj.setValue = 123;
    console.log(obj.value);
    //결과
    123

8.4와 8.5가 ES6의 방식이며, 더 직관적이고 가독성을 높일 수 있다.

### 8.6 is(): 값과 값 타입 비교

두 개의 파라미터 값과 값 타입을 비교하여 같음연 true를, 아니면 false를 반환한다.

1. === : 값과 값 타입 모두를 비교한다.
2. == : 값 타입은 비교하지 않고 값만 비교한다.
3. Object.is() : 값과 값 타입을 모두 비교한다.

=== 과 Object.is()는 비교 기준이 같다. 다만 +0과 -0을 비교하면 is는 false, ===는 true를 반환한다. NaN과 NaN을 비교하면 is는 true를 반환하고 ===는 false를 반환한다.

## 8.7 assign(): 오브젝트 프로퍼티 복사

두 번째 파라미터의 오브젝트 프로퍼티를 첫 번째 파라미터의 오브젝트 끝에 복사하고, 첫 번째 파라미터를 반환한다.

    try {
        let obj = Object.assign(null, {x: 1});
    } catch(e) {
        console.log("null 지정불가");
    }
    console.log(Object.assign(123));
    console.log(Object.assign(456, 70));
    //결과
    null 지정불가
    Number {[[PrimitiveValue]]: 123}
    Number {[[PrimitiveValue]]: 456}

    let oneObj = {};
    Object.assign(oneObj, "ABC", undefined, null);
    console.log(oneObj);

    let twoObj = {};
    Object.assign(twoObj, {key1: undefined, key2: null});
    console.log(twoObj);

    //결과

    Object {0: "A", 1: "B", 2: "C"}
    Object {key1: undefined, key2: null}

## 8.8 assign()의 필요성

Object 오브젝트를 변수에 할당하면 프로퍼티가 연동되어 한 쪽의 프로퍼티 값을 바꾸면 다른 한 쪽의 프로퍼티 값이 자동으로 바뀐다. 상황에 따라 다르겠지만, 연동되면 안 되는 경우 Object.assign()으로 복사하면 프로퍼티 값이 연동되지 않는다.

    let sports = {
        event: "축구",
        player: 11
    }
    let dup = sports;

    sports.player = 55;
    console.log(dup.player);

    dup.event = "농구"
    console.log(sports.event);
    //결과
    55
    농구
와 같이 연동되어 버린다.

    let sports = {
        event: "축구",
        player: 11
    }

    let dup = {};
    for(var i in sports){
        dup[key] = sports[key];
    }
    sports.player = 33;

    console.log(dup.player);

    //결과
    11

ES5에서 사용하는 방법이다.

    let sports = {
        event: "축구",
        player: 11
    }

    let dup = Obejct.assign({}, sports);
    console.log(dup.player);

    dup.player = 33;
    console.log(sports.player);

    sports.event("수영");
    console.log(dup.event);

    //결과
    11
    11
    축구

ES6에서는 다음과 같이 assign을 이용한다.

## 8.9 assign() 고려사항

    let oneObj = {one: 1}, twoObj = {two: 2};
    let mergeObj = Object.assign(oneObj, twoObj);

    console.log(Object.is(oneObj, mergeObj));

    mergeObj.one = 456;
    console.log(Object.is(oneObj, mergeObj));
    //결과
    true
    true

Object.assign 으로 복사한 oneObj와 twoObj의 프로퍼티는 연동되지 않지만, oneObj를 할당한 오브젝트 mergeObj는 연동된다.

