# **Ch6. 디스트럭쳐링**

## 6.1 개요
다음과 같은 형태가 디스트럭처링이다.

    let one, two;
    [one, two] = [1, 2];

이 코드를 실행하면 one 변수에 1이 할당되고, two 변수에 2가 할당된다. 배열 [1, 2]를 엘리먼트로 분할하여 one과 two 변수에 할당한다. ES6 스펙에서는 이를 Destructuring Assignment로 표기한다.

Destructure 의 사전적 표기는 구조를 파괴하다 인데, ES6의 디스트럭쳐링과는 거리가 있다. 오른쪽의 배열을 엘리먼트로 분할하고 엘리먼트 값을 왼쪽 변수에 해당하는 기능 측면에서 보면 파괴보다는 분할 할당이 더 어울린다.

## 6.2 Array 분할 할당

    let one, two, three, four, five;
    const values = [1, 2, 3];

    [one, two, three] = values;
    console.log("A:", one, two, three);

    [one, two] = values;
    console.log("B:", one, two);

    [one, two, three, four] = value;
    console.log("C:", one, two, three, four);

    [one, two, [three, four]] = [1, 2, [73, 74]];
    console.log("D:", one, two, three, four);

    //결과
    A: 1 2 3
    B: 1 2
    C: 1 2 3 undefined
    D: 1 2 73 74

마지막의 예제는 2차원 배열을 할당한다. 차원이 같으므로 할당에 문제가 없다. 하지만 차원이 같지 않다면 에러가 발생한다.

    let one, two, three, four, other;
    [one, , , four] = [1, 2, 3, 4];
    console.log(one, four);

    [one, ...other] = [1, 2, 3, 4];
    console.log(other);

    //결과
    1, 4
    [2, 3, 4]

## 6.3 Object 분할 할당
오른쪽 Object 오브젝트를 프로퍼티 단위로 분할하고 프로퍼티 키와 이름이 같은 왼쪽 변수에 값을 할당한다.

    let {one, two} = {one: 1, nin: 9};
    console.log(one, two);
    
    let three, four;
    ({three, four} = {three: 3, four: 4});
    console.log(three, four);

    //결과
    1 undefined
    3 4

오브젝트 할당에서 사전에 선언된 변수를 사용하려면, 소괄호 ( ) 안에 할당 코드를 작성하여야 한다.

    let five, six;
    ({one: five, two: six} = {one: 10, two: 20});
    console.log(five, six);

    let {nine, plus: {ten}} = {nine: 9, plus: ten: {10}};
    console.log(nine, ten);

    //결과
    10 20
    9 10

왼쪽과 오른쪽 오브젝트에 one이 있다. 이 형태에서 one은 변수가 아닌 프로퍼티 키다. two도 마찬가지다. 프로퍼티 키 값이 같은 five 변수에 10을, six 변수에 20을 할당한다.

## 6.4 파라미터 분할 할당

    function totla({one, plus: {two, five}}) {
        console.log(one + two + five);
    };
    total({one: 1, plus: {two: 2, five: 5}});

    //결과
    8

