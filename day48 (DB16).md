# [오전수업] DB 16차
## CONCAT 함수
- 입력한 두개의 문자열값을 하나로 합친 결과를 연산하는 함수

```
SELECT CONCAT('Hello', 'World'), 'Hello' || ' Hell ' ||'World'
FROM dual;

CONCAT('HELLO','WORLD')|'HELLO'||'HELL'||'WORLD'|
-----------------------+------------------------+
HelloWorld             |Hello Hell World        |
```

- 함수를 중첩하여 사용하는 것도 가능
- 다만 중첩되는 함수의 입력값의 데이터 타입이나, 출력하는 값이 맞아야 문제없이 연산 가능
- 중첩된 함수의 실행 순서는 안쪽의 함수가 먼저 실행되고 실행된 결과를 바깥의 함수로 전달 후 바깥의 함수가 실행
```

SELECT CONCAT(CONCAT('Hello', ' HELL '), 'World'), 'Hello' || ' Hell ' ||'World'
FROM dual;

CONCAT(CONCAT('HELLO','HELL'),'WORLD')|'HELLO'||'HELL'||'WORLD'|
--------------------------------------+------------------------+
Hello HELL World                      |Hello Hell World        |
```

## SUBSTR 함수
- 문자열의 일부를 추출하여 출력하는 함수
- SUBSTR('전체문자열', 추출시작자리값, 자를칸수)

```
- 'HelloWorld'의 1번째 자리에서부터 5칸을 추출
SELECT SUBSTR('HelloWorld', 1, 5)
FROM dual;

SUBSTR('HELLOWORLD',1,5)|
------------------------+
Hello                   |

```

- 자리값은 왼쪽 → 오른쪽으로 세는 경우 양수, 오른쪽 → 왼쪽으로 세는 경우 음수로 사용

```
오른쪽 끝에서 3번째 자리에서 2칸을 추출

SELECT SUBSTR('HelloWorld', -3, 2)
FROM dual;

SUBSTR('HELLOWORLD',-3,2)|
-------------------------+
rl                       |
```

