# **Ch.7 오퍼레이션**

## 7.1 프로퍼티 이름 조합

문자열과 변수를 조합하여 오브젝트의 프로퍼티 이름으로 사용할 수 있다. 이를 Compound property name 이라고 한다.

### 문자열 조합
    let item = {
        ["one" + "two"]: 12
    };
    console.log(item.onetwo);
    //결과
    12

### 변수 값과 문자열 조합

    let item = "tennis";
    let sports = {
        [item]: 1,
        [item + "Game"]: "윔블던",
        [item + "Method"](){
            return this[item];
        }
    };
    console.log(sports.tennis);
    console.log(sports.tennisGame);
    console.log(sports.tennisMethod());
    
    //결과
    1
    윔블던
    1
### 디스트럭쳐링과 프로퍼티 이름 조합

    let one = "sports";
    let {[one]: value} = {sports: "농구"};
    consple.log(value);
    //결과
    농구

## 7.2 Default Value

변수, 파라미터, 프로퍼티에 값이 할당되지 않을 때 사전에 정의한 값이 할당된다. 이를 default value라고 한다. 따라서 변수, 파라미터, 프로퍼티가 항상 값을 가진다.

일반적으로 사용하는 디폴트 값과는 차이가 있다. let 변수를 선언하면 디폴트 값으로 undefined가 설정된다. 반면 여기의 default value는 let 변수를 선언하면서 값이 설정되지 않았을 때 할당된 값을 작성해 두는 형태다.

    let [one, two, five = 5] = [1, 2];
    console.log(five);

    [one, two, five = 5] = [1, 2, 77];
    console.log(five);

    let [one, two, five = 5] = [1];
    console.log(two);

    let {six, seven = 7} = {six: 6};
    console.log(six, seven);

    //결과
    5
    77
    undefined
    6 7

### 디폴트 값 적용 순서
왼쪽에서 오른쪽으로 디폴트 값이 적용된다.

    let [one, two = one + 1, five = two + 3] = [1];
    console.log(one, two, five);

    //결과
    1
    2
    5

## 7.3 Default 파라미터
호출 받는 함수의 파라미터에 디폴트 값을 작성할 수 있다. 호출하는 함수에서 파라미터 값을 넘겨주지 않거나 undefined를 넘겨주면 디폴트 값이 적용된다.

    let plus = (one, two = 2) => one + two;

    console.log(plue(1));
    console.log(plue(1, undefined));
    console.log(plue(1, 70));

    //결과
    3
    3
    71

### 파라미터 디스트럭처링
    
    let getTotal = ([one, two] = [10, 20]) => one + two;
    console.log(getTotal());

    let getValue = ({two: value} = {two: 20}) => value;
    console.log(getValue());

    //결과
    30
    20

## 7.4 for-of

반복한다는 점은 `for-in`과 차이가 없지만 대상과 방법에서 차이가 있다.

    for(var value of [10, 20, 30]){
        console.log(value);
    }
    //결과
    10
    20
    30

### String 사용
    for(var value of "ABC"){
        console.log(value);
    }
    //결과
    "A"
    "B"
    "C"

### NodeList 반복

    <script src="for-of-3.js" defer></script>
    <ul>
        <li>첫 번째</li>
        <li>두 번째</li>
        <li>세 번째</li>
    </ul>


    //for-of-3.js
    let nodes = document.querySelectorAll("li");
    for(var node of nodes){
        console.log(node.textContent);
    }

    //결과
    첫 번째
    두 번째
    세 번째
### 디스트럭처링

    let values = [
        {items: '선물1', amount: {apple: 10, candy: 20}},
        {items: '선물2', amount: {apple: 30, candy: 40}}
    ];
    for(var {item: one, amount {apple: two, candy: five}} of values){
        console.log(one, two, five);
    }
    //결과
    선물1 10 20
    선물2 30 40

## 7.5 for-of 와 for-in의 차이
for-in 문의 대상은 Object 오브젝트이며 열거 가능한 프로퍼티가 대상이다. 즉 프로퍼티의 enumerable 속성 값이 false 이면 반복에서 제외된다. for-of 문의 대상은 이터러블 오브젝트이며 prototype에 연결된 프로퍼티는 대상이 아니다.

    let values = [10, 20, 30];
    Array.prototype.music = function(){
        return "음악";
    }
    Object.prototype.sports = function(){
        return "스포츠";
    }
    for(var key in values){
        console.log(key, values[key]);
    }
    console.log("<<<for-of>>>");
    for(var value of values){
        console.log(value);
    }

    //결과
    0 10
    1 20
    2 30
    music function(){return "음악"}
    sports function(){return "스포츠"}
    <<<for-of>>>
    10
    20
    30

## 7.6 for-of로 Object 열거
Object 오브젝트는 이터러블 오브젝트가 아니므로 for-of 문으로 열거할 수 없습니다. 한편 개발자 코드로 사전 처리를 하면 for-of 문을 열거할 수 있습니다.
    let sports = {
        soccer: '축구',
        baseball: '야구'
    }
    let keyList = Object.keys(sports);
    for(var key of keyList){
        console.log(key, sports[key]);
    }

    //결과
    soccer 축구
    baseball 야구
Object.keys(sports)는 파라미터의 sports 오브젝트에서 프로퍼티 키를 배열로 반환한다. 배열은 이터러블 오브젝트이므로 이를 for-of 문으로 반복하면서 sports[key] 형태로 프로퍼티 값을 구할 수 있다.

## 7.7 거듭 제곱 연산자
    console.log(3**2);
    console.log(3**3);
    console.log(Math.pow(3, 3));
    //결과
    9
    27
    27