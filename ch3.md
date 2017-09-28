# Ch3. arrow 함수

## 개요
    (param) => { code };
    param => { code };
    () => { code };
    (param1, param2) => { code };
    (param1, param2 = 123, 4324) => { code };
    ([one, two] = [1, 2]) => one + two;
    ({key: sum} = {key: 10 + 20}) => { code }; 

- arrow 함수는 function(param){ code } 형태를 축약한 것으로 (param) => { code } 형태로 작성한다.
- arrow 함수는 함수 이름이 없는 무명/익명 함수이다. 따라서 함수를 호출하려면 ```let fn = (param) => { code }``` Function 오브젝트를 할당할 변수를 작성해야 한다.
- arrow 함수는 function 키워드보다 간단하게 함수를 선언할 수 있어 편리하다. 하지만 모든 경우에 사용할 수 없으며 함수가 실행되는 환경을 고려해야한다.(뒤에 나올 내용을 보자)

## ES5 형태와 차이.

    var es5 = function(one, two){
        return one + two;
    }
    var sum = es5(1, 2);
    console.log(sum);
    //es5

    let es6 = (one, two) => {
        return one + two;
    }
    let result es6(1, 2);
    console.log(result);
    //es6


>       let es6 = (one, two) => {
>            return one + two;
>        } 
위 코드는 다음의 순서와 방법으로 작성되었다.

1. arrow 함수로 생성한 Function 오브젝트를 할당할 변수(es6)을 작성한다.
2. function 키워드를 사용하지 않는다.
3. ( ) 안에 파라미터 이름을 작성한다.
4. 이어서 arrow 를 작성한다.
5. 화살표 앞뒤에 공백을 띄어도 되고 붙여도 된다.
6. 함수 블록 { }을 작성하고 블록 안에 코드를 작성한다.

## 블록, 파라미터
    let total = (one, two) => one + two;
    let reulst = total(1, 2);
    //{ } 을 작성하지 않고 그냥 한 줄에 작성하여도 된다.

## return

arrow 함수에서 {key: value}  형태의 Object를 반환하려면 ( )안에 {key: value}를 작성한다.

    let sports = () => { };
    result = sports();
    console.log(result);
    //undefined
위 코드의 목표는 sports( ) 함수를 호출하면, {key: value} 형태의 빈 Object를 반환하는 것이 목적이다. 하지만 undefined가 반환된다.

이는 => 다음의 { } 를 함수 블록으로 인식하고, 함수 블록 안에 return 문을 작성하지 않은 것으로 인식하기 때문이다. 함수에 return 문을 작성하지 않으면 디폴트로 undefined가 반환된다. arrow 함수이기 때문에 undefined가 반환되는 것이 아니다.

    let get = param => ({sports: 축구});
    let result = get();
    console.log(result);
    //축구

## new 연산자

arrow 함수는 new 연산자로 인스턴스를 생성할 수 없다.

## arguments 사용

    let sports = () => {
        try {
            let args = arguments;
        } catch(error){
            console.log("error");
        }
    }
    sports(1,2);

    //"error"

function 키워드로 선언한 함수를 sports(1, 2) 형태로 호출하면 함수의 arguments에 1과 2가 설정된다. 반면 화살표 함수에는 arguments가 존재하지 않는다. 블록에서 arguments를 사용하면 ReferenceError가 발생한다.

ES6에서는 arguments 대신에 rest 파라미터를 사용한다. rest 파라미터는 ```let sports = (...rest) => { code } ``` 형태와 같이 ( )안에 점(.)을 3개 작성하고 이어서 파라미터 이름을 작성한다. sports(1, 2)로 호출하면 1과 2가 rest 파라미터에 배열로 설정된다.

## this와 setTimeout()

화살표 함수가 간단하게 코드를 작성할 수 있어 편리하지만 this의 참조를 고려해야 한다.

    let Sports = function(){
        this.count = 20;
    };
    Sports.prototype = {
        plus: function(){
            this.count += 1;
        },
        get: function(){
            setTimeout(function(){
                console.log(this === window);
                console.log(this.plus);
            }, 1000);
        }
    };
    let newSports = new Sports();
    newSports.get();
결과는 true와 undefined

    let newSports = new Sports();
new 연산자로 Sports 인스턴스를 생성하여 newSports 변수에 할당한다. Sports 생성자 함수에서 this.count에 20을 할당한다. 이때 this는 생성하는 인스턴스를 참조하므로 count 프로퍼티에 20이 설정된다.

    get: function(){
        setTimeout(function(){

        }, 1000);
    }

newSports.get()을 호출하면, get( )함수에 작성된 setTimeOut() 함수가 실행되며 1초 후에 콜백 함수가 실행된다.

    console.log(this === window);

setTimeout()이 window 오브젝트 함수이므로 this가 window 오브젝트를 참조하게 된다. 그래서 콘솔에 true가 출력된다. 여기서 문제는 newSports.get() 형태로 호출했으므로 this가 newSports 인스턴스를 참조해야 하는데 window 오브젝트를 참조한다는 점이다.

ES5에서 this가 newSports 인스턴스를 참조하지 못하므로 setTimeout()을 실행하기 전에 newSports 인스턴스를 변수에 할당하고, setTimeout() 콜백 함수에서 변수의 인스턴스를 사용하는 형태를 취했다.

    console.log(this.plus);

this.plus의 목적은 this로 newSports 인스턴스를 참조하여 plus 메서드를 반환받는 것이다. 그런데 this가 window 오브젝트를 참조하므로 콘솔에 undefined가 출력된다. 화살표 함수를 사용하여 이 문제를 해결할 수 있다.

## arrow 함수와 setTimeout()

setTimeout() 함수의 콜백 함수를 화살표 함수로 작성하면 this가 setTimeout()이 포함된 함수의 인스턴스를 참조한다.

    let Sports = function(){
        this.count = 20;
    };
    Sports.prototype = {
        plus: function(){
            this.count += 1;
        },
        get: function(){
            setTimeout(() => {
                this.plus();
                console.log(this.count);
            }, 1000);
        }
    };
    let newSports = new Sports();
    newSports.get();

결과는 21

위의 코드와 같다. 다만 setTimeout() 안에 arrow 함수가 들어왔다는 점이 다르다.

newSports.get() 을 호출하면, get()에 작성된 setTimeout이 실행되며 1초 후에 콜백 함수가 실행된다. 화살표 함수 블록에서 this가 newSports.get() 형태의 newSports 인스턴스를 참조한다.
this가 newSports 인스턴스를 참조하므로 plus()가 호출된다. 따라서 결과 값은 21이 출력된다.

## prototype

prototype에 arrow 함수를 연결하면 arrow 함수 블록에서 this가 인스턴스를 참조하지 못한다.

    let Sports = function(){
        this.count = 20;
    };
    Sports.prototype = {
        add: () => {
            this.count += 1;
        }
    };

    let newSports = new Sports();

    newSports.add();
    console.log(newSports.count);
    console.log(window.count);

실행결과는 20 Nan

newSports 인스턴스의 add()를 호출하므로 add()에서 this로 newSports 인스턴스를 참조할 수 있다. add()에서 this.count에 1을 더하므로 newSports.count에서 21을 출려해야 하는데 20이 출력된다.

add()는 arrow 함수다. newSports.add() 형태로 호출하므로 this.count에서 this가 newSports 인스턴스를 참조해야 하는데 window 오브젝트를 참조한다. 따라서 newSports 인스턴스의 count에 1을 더하지 않고 window 오브젝트의 count에 1을 더한다.

한편 window 오브젝트에 count 프로퍼티가 없어도 error가 발생하지 않는다. 오브젝트에서 프로퍼티 갑승ㄹ 구할 때 프로퍼티가 없으면 undefined를 반환한다. this.count += 1 코드는 window 오브젝트에서 count 프로퍼티 값을 구하고, 여기에 1을 더해 다시 설정한다. count 프로퍼티가 없으므로 undefined를 반환하게 되고, 여기에 1을 더하므로 Nan(Not a Number)가 출력된다. 이는 자바스크립트의 보편적인 처리 방법이다.

지금껏 보았듯이 prototype에 화살표 함수를 연결하면, this가 메서드를 호출한 인스턴스를 참조하지 않고 window 오브젝트를 참조한다. 따라서 화살표 함수가 아닌 function 키워드 함수를 prototype에 연결해야 한다.