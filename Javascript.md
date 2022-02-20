> 인터넷 강의 "웹 앱 개발을 위한 Javascript 기초" 의 내용을 요약하여 간단히 정리함.

# 자바스크립트 기본 환경 설치 및 기본 문법
## 환경 설치
### 기반환경 구축
- 자바스크립트 : HTML, CSS와 함께 클라이언트 측 웹프로그래밍을 할 수 있도록 돕는 프로그래밍 언어
- 자바스크립트는 인터프리터(interpreter)언어로 작성과 동시에 바로 실행할 수 있는 언어
  - 일반적으로 프로그래밍은 컴파일러와 인터프리터라는 과정을 통하여 프로그래밍 진행
- 커맨드 라인 인터페이스(CLI: Command Line Interface) 활용, 에디터의 활용
  - WebStorm, Visual Studio Code, nods.js, npm 등등

## 기본 문법 구성
- 변수 : 프로그램을 실행하는 도중에 임의의 값을 저장해 두고 읽을 수 있는 가상의 공간
  - 데이터타입 : 변수에 저장할 수 있는 값의 종류를 데이터 타입 또는 자료형이라고 함
    - 숫자형(number type), 문자열(string type), 불린형(boolean tye) 등이 존재
    - typeof() 명령어를 사용하면 소괄호 안에 들어 있는 변수의 데이터타입 확인 가능
- 연산자와 함수 : 숫자를 계산하는 연산자(계산하는 기호)를 산술 연산자라 하고 함수는 수학에서 사용하는 함수의 개념하고 비슷함
- 문자열 : 문자에 대한 길이나 두 문자열을 이어 붙이는데 사용함
- 배열 : 여러 값이 연속으로 저장된 공간을 뜻함
- 주석 : 프로그램이 동작하는 데는 전혀 관련이 없는 코드, 보통은 개발자들의 편의와 가독성을 높이기 위해 사용
- 조건문 : 조건에 따라 프로그램을 분기하도록 만들어줌
- 반복문 : 컴퓨터에게 반복작업을 시킬 수 있는 명령


# 기본 명령어 및 연산자
## 기본 명령어
- 표현식, 문장, 프로그램
  - 표현식이 하나 이상 모였을 경우 마지막에 종결의 의미로 세미콜론(;)을 찍어주어 문장을 완성시킴.
  - 문장이 모이면 프로그램이 됨. 
- 예약어 : 자바스크립트 문법을 규정짓기 위해 자바스크립트 언어 사양에서 사용하는 특수한 키워드는 식별자로 사용하지 않는 편이 좋음
  - break, case, class, for, this, if, new, return, true 등등 
- 식별자 : 이름을 붙일 때 사용하는 단어, 변수와 함수 이름 등으로 사용
```
  - 식별자 작성 규칙
    - 특수문자는 _와 $만 허용
    - 숫자로 시작하면 안 됨
    - 공백은 입력하면 안 됨
    - 생성자 함수의 이름은 항상 대문자로 시작
    - 변수, 함수, 속성, 메소드의 이름은 항상 소문자로 시작
    - 여러 단어로 구성된 식별자는 각 단어의 첫 글자를 대문자로 함(ex: i am a boy -> iAmABoy)  
```    
- 문자열 : 자바스크립트를 HTML 요소에 끼워 넣을 때는 자바스크립트 프로그램을 문자열로 작성
  - 문자열 작성 시 큰 따옴표나 작은 따옴표 사용 
  - 다양한 이스케이프 문자가 존재
- 변수 : 값을 저장할 때 사용하는 식별자, 변수 선언 후 변수에 값을 할당

## 연산자 
- 사칙연산(+, -, *, /)
- 비교 연산자(==, !=, >, <, >=)
  - != : 다르다 
- 논리 연산자(!, ||, &&)
  - ! : 논리 부정 연산자
  - || : 논리합 연산자
  - && : 논리곱 연산자 
- 복합 대입 연산자(+=, -=, *=, /=)
  - += : 숫자 덧셈 후 대입 연산자
  - -= : 숫자 뺄셈 후 대입 연산자
  - *= : 숫자 곱셈 후 대입 연산자
  - /= : 숫자 나눗셈 후 대입 연산자
- 증감 연산자
  - 변수 number를 초기화하고 ++연산자와 --연산자 사용
    - 각 연산자에서 변수 값이 1만큼 변경됨 
- 자료형 확인 연산자
  - typeof : 해당 변수의 자료형을 추출
- 강제 자료형 변환
  - Number() : 숫자로 자료형 변환
    - 숫자 자료형이지만 숫자가 아닌 것도 존재 (NaN)
  - String() : 문자열로 자료형 변환
  - Boolean() : 불린으로 자료형 변환

# 조건문과 연산자
## 조건문
- if 조건문
```javascript
if (<불린 표현식>) {
}
```

- if else 조건문
```javascript
if (<불린 표현식>) {
  불린 표현식이 참이 때 실행할 문장
} else {
  불린 표현식이 거짓일 때 실행하는 문장
}
```

- 중첩 조건문
```javascript
if (불린 표현식) {
  if (불린 표현식) {
  문장;
  } else {
  문장;
}
```

- switch 조건문
```javascript
switch {<불린 표현식>} {


  case <값> :
  <문장>
  break;
  case <값> :
  <문장>
  break;
  default :
  <문장>
  break;
}
```
> 아주 기본적인 문법만을 작성해 놓았으며, 다양하게 활용 가능.

## 연산자
- 배열 : 여러 개의 자료를 한꺼번에 다룰 수 있는 자료형
  - 배열의 요소 : 배열[인덱스]
  - 인덱스의 시작 숫자는 0임에 주의 
- while 반복문 : 특정한 숫자를 증가시켜 불 표현식을 false로 만들어 반복문을 벗어남
```javascript
While (<불린 표현식>) {
  불 표현식이 참인 동안 실행될 문장
}
```
  - 특정한 숫자를 증가시켜 불린 표현식을 false로 만들어 반복문을 벗어남
- for 반복문 : 초기식∙조건식 비교, 조건이 false이면 반복문을 종료, 문장∙종결식 실행, 2단계로 이동
```javascript
for (var i = 0; i < (반복횟수); i++) {
  문장 실행
}
```
- 역 for 반복문 : 배열의 요소를 뒤쪽부터 출력
```javascript
for (var i = length - 1; i >= 0; i--) {
  문장 실행
}
```
- for in 반복문과 for of 반복문 : for in 반복문과 for of 반복문은 for 반복문 사용과 역할이 같음
```javascript
for (var 인덱스 in 배열) {

}
---------------------------
for (var 요소 of 배열) {

}
```
- 중첩 반복문 : 반복문을 여러 번 중첩해서 사용
- break 키워드(반복문을 벗어날 때 사용), continue 키워드(반복문 내부에서 현재 반복을 멈추고 다음 반복을 진행함)


# 자바스크립트 함수
## 함수 기본
### 함수의 생성 방법
- 익명 함수 : 이름을 붙이지 않고 함수 생성
- 선언적 함수 : 이름을 붙여 함수를 생성
- 화살표 함수 : ‘하나의 표현식을 리턴하는 함수’를 만들 때는 중괄호 생략 가능
- 함수의 기본 형태에서부터 다양하게 활용 가능
  - 기본 형태
```javascript
function 함수이름 (<매개변수>), {
식
}
```
  - 함수 매개 변수 초기화
```javascript
// 함수 선언
function print(name, count) {
  alert(name + count)
}

// 함수 호출
print("사과", 10)
```

  - 표준 내장 함수
    - parseInt() : 문자열을 정수로 변환
    - parseFloat() : 문자열을 실수로 변환
    - setTimeout(함수, 시간) : 특정 시간 후에 함수를 실행
    - setInterval(함수, 시간) : 특정 시간마다 함수 실행
    - clearInterval(아이디) : 특정 시간마다 실행하던 함수 호출을 정지
    - etc

  - 변수 덮어쓰기 
```javascript
var 변수;
변수 = 10;
변수 = 20;

alert(변수)
// 20이 출력
```
  - 함수 덮어쓰기
```javascript
var 변수;
변수 = function() {alert(첫 번째 함수)};
변수 = function() {alert(두 번째 함수)};

함수();
// 두 번째 함수가 실행
```

- 그 외에 콜백 함수 등 다양한 활용이 존재 

    
# 자바스크립트 객체
## 객체
- 객체 기본 : 객체 선언, 객체 접근, 객체 생성
  - 객체 선언
```javascript
let product = {
  제품명 : "7D 건조 망고",
  유형 : "당절임",
  성분 : "망고, 설탕, 메타중아황산나트륨, 치자황색소",
  원산지 : "필리핀"
};
```
  - 객체 접근
```javascript
product['제품명'] -> '7D 건조 망고'
product['유형'] -> '당절임'

product.성분 -> '망고, 설탕, 메타중아황산나트륨, 치자황색소'
product.원산지 -> '필리핀'
```

- 객체와 반복문 : for in 반복문을 사용해 객체에 반복문을 적용
```javascript
let object = {
  name : "바나나"
  price : 12000
};

for (let key in object) {
  console.log(`${key}: ${object[key]}`);
}

```
- 속성과 메소드 : 요소(배열 내부에 있는 값 하나하나), 속성(객체 내부에 있는 값 하나하나)
  - 객체의 다양한 자료형
```javascript
var object = {
  number : 273,
  string : 'RintlanTta',
  boolean : true,
  array : [52, 273, 103, 32]
  method : functon() {
  
  }
  
};
```

  - 메소드(객체의 속성 중 자료형이 함수인 속성)
```javascript
let object = {
  name : '바나나',
  price : 1200,
  print : function() {
    console.log(`${this.name}의 가격은 ${this.price}원입니다.`)
  }
};

Object.print();
```

- 생성자 함수와 프로토타입 
  - 객체 지향 프로그래밍
    - 현실의 객체를 모방해서 프로그래밍 
  - 배열과 객체를 사용하면 여러 개의 데이터를 쉽게 다룰 수 있음
  - 생성자 함수: 객체를 만드는 함수
  - 프로토타입 : 생성자 함수로 만든 객체는 프로토타입 공간에 메소드를 지정해서 모든 객체가 공유하도록 함

## 표준 내장 객체
- 기본 자료형과 객체 자료형의 차이 : 자바스크립트는 다양한 객체를 제공, 기본 자료형의 속성 또는 메소드를 사용할 때 기본 자료형이 자동으로 객체로 변환이 됨, 기본 자료형은 객체가 아니므로 속성과 메소드를 추가할 수 없음
- Number 객체 : 자바스크립트에서 숫자를 표현할 때 사용
```
메소드
  - toExponential() : 숫자를 지수 표시로 나타낸 문자열 리턴
  - toFixed() : 숫자를 고정소수점 표시로 나타낸 문자열 리턴
  - toPrecision() : 숫자를 길이에 따라 지수 표시 또는 고정소수점 표시로 나타낸 문자열 리턴
속성
  - MAX_VALUE : 자바스크립트의 숫자가 나타낼 수 있는 최대 숫자
  - MIN_VALUE : 자바스크립트의 숫자가 나타낼 수 있는 최소 숫자
  - NaN : 자바스크립트의 숫자가 나타낼 수 없는 숫자
  - POSITIVE_INFINITY : 양의 무한대 숫자
  - NEGATIVE_INFINITY : 음의 무한대 숫자
```
- String 객체 : String 객체의 메소드는 변경된 값을 리턴함
```
메소드
  - charAr, concat, match, searchc, slice ,split, substring 등 다양한 메소드 존재
속성
  - length : 문자열의 길이를 나타냄
```  
- Date 객체
```
생성자 함수 
  - new Date() : 현재 시간으로 Date 객체 생성
  - new Date(유닉스 타임) : 유닉스 타임(1970년 1월 1일 00시 00분 00초부터 경과한 밀리초)로 Date 객체 생성
  - new Date(<시간 문자열>) : 문자열로 Date 객체 생성
  - new Date(<년>, <월-1>, <일>, <시간>, <분>, <초>, <밀리초>) : 시간 요소를 기반으로 Date 객체 생성
```


# 객체 모델  
## 브라우저 객체
- window 객체
  - 대표적인 객체 모델 : location, navigator, history, screen, document 객체
  - 많은 속성이 있음, 일부 익스플로러에서는 실행되지 않음
  - 자신의 형태와 위치를 변경할 수 있도록 메소드 제공
    - moveBy(x,y), moveTo(x,y), resizeBy(x,y), resizeTo(x,y), scrollBy(x,y), scrollTo(x,y), focus(), blur(), close() 등
    - By : 상대적 / To : 절대적
- screen 객체 : 웹 브라우저의 화면이 아닌 운영체제 화면의 속성을 가지는 객체
  - width, height, colorDepth, pixelDepth 등 
- location 객체
  - 브라우저의 주소 표시줄과 관련된 객체
  - 프로토콜의 종류, 호스트 이름, 문서 위치 등의 정보가 있음
    - href, host, port, hash, search, protocol, reload(), replace() 등
- navigator 객체 
  - 웹 페이지를 실행하고 있는 브라우저에 대한 정보가 있음
  - appCodeName, appName, platform, userAgent 등
- window 객체의 onload 이벤트 속성 : 문서 객체 속성 중‘on’으로 시작하는 속성을 이벤트 속성이라 부르고 함수를 할당해야 함
```javascript
<script>
  window.onload = function() {
  
  };
</script>
```

## 문서 객체
- 넓은 의미 : 웹 브라우저가 HTML 페이지를 인식하는 방식
- 좁은 의미 : document 객체와 관련된 객체의 집합
- 노드
  - 요소 노드 : HTML 태그
  - 텍스트 노드 : 요소 노드 안에 들어 있는 글자들   
- 문서 객체 
  - 텍스트 노드를 갖는 문서객체 : 요소 노드와 텍스트 노드 생성 후 텍스트 노드를 요소 노드에 붙여 줌
  - 텍스트 노드를 가지지 않는 문서객체 : 대표적으로 img 태그
- 문서 객체 가져오기
  - getElementById(id), getElementByName(name), getElementByTagName(tagName) 등의 메소드를 이용
  - 가져온 문서 객체의 스타일 조작, style 속성의 사용이 가능함
    - style 속성 : border, color, fontFamily 등
- 문서 객체 제거 : 제거 메소드 사용
```javascript

<body>
  <h1 id = "will-remove">Header</h1>
</body>  


<script>
  window.onload = function() {
  // 문서 객체 가져오기
  var willRemove = document.getElementById('will-remove')
  
  // 문서 객체 제거하기
  document.body.removeChild(willRemove);
};
</script>
</body>
```
