# Ch.9 Number

## 9.1 Number 상수

두개의 Number 상수가 추가되었다.

    console.log(Number.MAX_SAFE_INTEGER);
    console.log(Number.MIN_SAFE_INTEGER);

    //결과
    9007199254740991 (253 - 1)
    - 9007199254740991 (-(253 - 1))

## 9.2 EPSILON

구글링

## 9.3 진수 리터럴
ES6에서 2진수 표기법이 추가되었으며 8진수 표기법이 재정의되었다.

### 2진수
첫 번째에 숫자 0을 작성하고 두 번째에 소문자 b 또는 대문자 B를 작성한다. 세 번째부터는 값을 0 또는 1로 작성한다.

0b0101 또는 0B0101 형태로 값은 5이다.

### 8진수
첫번째 숫자 0을 작성하고 두 번째에 o 또는 O를 작성. 세번째 부터 0부터 7까지 값을 작성한다.
0o0105 또는 0O0105 의 형태다 값은 65다.

## 9.4 isNAN()
파라미터 값이 NaN(Not a Number)이면 true를 반환 아니면 false를 반환.

## 그 외 함수에 관한 설명 이어진다. 구글링.