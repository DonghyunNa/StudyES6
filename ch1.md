# **Ch1. let, const**

## let 키워드

    let sports = "축구";

```let``` 키워드는 ```var``` 키워드의 문제점을 해결하기 위한 것으로 다음과 같은 특징을 가진다.

- 함수 안에 작성한 ```let``` 변수는 함수가 스코프이다.
- 함수 안에 ```if(a = b){let sports = "축구"}``` 형태로 코드를 작성했을 때, ```sports``` 변수는 함수가 스코프가 아니라 if문의 블록 { }이 스코프이다.
- 블록 { } 밖에 같은 이름의 변수가 있어도 스코프가 다르므로 변수 각각에 값을 설정할 수 있으며 변수 값이 유지된다.
- 블록 안에 블록을 계층적으로 작성하면 각각의 블록이 스코프이다.
- 같은 스코프에서 같은 이름의 ```let```변수를 선언할 수 없다.
- ```let``` 변수는 hoisting되지 않는다.

### 예시

    let sports = "축구";
    if(sports){
        let sports = "농구";
        console.log("블록: ", sports);
    }
    console.log("글로벌: ", sports);
실행결과는 

    블록: 농구
    글로벌: 축구

## let과 this 키웓,

    var music = "음악";
    console.log(music);

    let sports = "축구";
    console.log(sports);

실행결과

    음악
    undefined

var 키워드로 music 변수를 선언하고 값을 할당. 스코프는 글로벌 오브젝트이고 this가 글로벌 오브젝트를 참조한다. ```console.log``` 에서는 글로벌 오브젝트의 music 변수 값인 "음악"이 출력된다.<br>

let 키워드로 변수를 선언하고 "축구"를 할당한 후, this.sports로 sports 변수 값을 출력하면 undefined가 출력된다. this가 window 오브젝트를 참조하는데 window 오브젝트에 let 변수가 없다는 것은 window 오브젝트에 let 변수가 설정되지 않는다는 의미가 된다. 이 점이 변수와 let 변수의 차이다.

> 무슨 뜻인지 추가 이해가 필요함..

