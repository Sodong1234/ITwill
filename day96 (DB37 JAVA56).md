# [오전수업] DB 37차

> 36차에서 실시한 연습문제 풀이 이어서 실시

```sql
p.75_3

SELECT job_id                                 "Job",
       SUM(decode(department_id, 20, salary)) "Dept 20",
       SUM(decode(department_id, 50, salary)) "Dept 50",
       SUM(decode(department_id, 80, salary)) "Dept 80",
       SUM(decode(department_id, 90, salary)) "Dept 90",
       SUM(salary)                            "Total"
FROM employees
GROUP BY job_id;
```

---

## 집합 연산자
### 합집합(UNION)
- 서로 다른 쿼리 구문의 결과를 합쳐서 출력하는 명령어
- 출력 시 출력하는 쿼리구문의 컬럼의 데이터 타입은 일치해야함
- 컬럼명은 첫번째 쿼리구문의 컬럼명을 출력
- UNION은 결과값을 정렬하고 중복값을 제거한 상태로 출력

```sql
SELECT employee_id,
       last_name,
       first_name,
       salary,
       department_id
FROM employees
WHERE salary > 10000
      AND department_id = 80;


EMPLOYEE_ID|LAST_NAME|FIRST_NAME|SALARY|DEPARTMENT_ID|
-----------+---------+----------+------+-------------+
        145|Russell  |John      | 14000|           80|
        146|Partners |Karen     | 13500|           80|
        147|Errazuriz|Alberto   | 12000|           80|
        148|Cambrault|Gerald    | 11000|           80|
        149|Zlotkey  |Eleni     | 10500|           80|
        162|Vishney  |Clara     | 10500|           80|
        168|Ozer     |Lisa      | 11500|           80|
        174|Abel     |Ellen     | 11000|           80|


-----------------------------------------------------------------------------

SELECT employee_id,
       last_name,
       first_name,
       salary,
       department_id
FROM employees
WHERE salary > 10000
      AND department_id = 90;

EMPLOYEE_ID|LAST_NAME|FIRST_NAME|SALARY|DEPARTMENT_ID|
-----------+---------+----------+------+-------------+
        100|King     |Steven    | 24000|           90|
        101|Kochhar  |Neena     | 17000|           90|
        102|De Haan  |Lex       | 17000|           90|

-----------------------------------------------------------------------------

SELECT employee_id,
       last_name,
       first_name,
       salary,
       department_id
FROM employees
WHERE salary > 10000
      AND department_id = 80
UNION
SELECT employee_id,
       last_name,
       first_name,
       salary,
       department_id
FROM employees
WHERE salary > 10000
      AND department_id = 90;


EMPLOYEE_ID|LAST_NAME|FIRST_NAME|SALARY|DEPARTMENT_ID|
-----------+---------+----------+------+-------------+
        100|King     |Steven    | 24000|           90|
        101|Kochhar  |Neena     | 17000|           90|
        102|De Haan  |Lex       | 17000|           90|
        145|Russell  |John      | 14000|           80|
        146|Partners |Karen     | 13500|           80|
        147|Errazuriz|Alberto   | 12000|           80|
        148|Cambrault|Gerald    | 11000|           80|
        149|Zlotkey  |Eleni     | 10500|           80|
        162|Vishney  |Clara     | 10500|           80|
        168|Ozer     |Lisa      | 11500|           80|
        174|Abel     |Ellen     | 11000|           80|



-----------------------------------------------------------------------------

컬럼 수가 맞지 않거나 출력할 내용이 없는 경우 NULL을 통해서 컬럼을 채울 수 있다.
SELECT manager_id,
       first_name,
       last_name,
       salary * 2,
       department_id
FROM employees
WHERE salary > 10000
      AND department_id = 80
UNION
SELECT department_id,
       department_name,
       NULL,
       manager_id,
       location_id
FROM departments;


MANAGER_ID|FIRST_NAME          |LAST_NAME|SALARY*2|DEPARTMENT_ID|
----------+--------------------+---------+--------+-------------+
        10|Administration      |         |     200|         1700|
        20|Marketing           |         |     201|         1800|
        30|Purchasing          |         |     114|         1700|
        40|Human Resources     |         |     203|         2400|
        50|Shipping            |         |     121|         1500|
        60|IT                  |         |     103|         1400|
        70|Public Relations    |         |     204|         2700|
        80|Sales               |         |     145|         2500|
        90|Executive           |         |     100|         1700|
       100|Alberto             |Errazuriz|   24000|           80|
       100|Eleni               |Zlotkey  |   21000|           80|
       100|Finance             |         |     108|         1700|
       100|Gerald              |Cambrault|   22000|           80|
       100|John                |Russell  |   28000|           80|
       100|Karen               |Partners |   27000|           80|
…

```

### UNION ALL
- UNION 과 동일하게 다른 쿼리 구문의 결과를 합쳐서 출력
  - 다만, 결과값에 대한 정렬이나 중복값의 제거는 하지 않음

```sql
SELECT employee_id,
       last_name,
       first_name,
       salary,
       department_id
FROM employees
WHERE salary > 10000
      AND department_id > 100
UNION ALL
SELECT employee_id,
       last_name,
       first_name,
       salary,
       department_id
FROM employees
WHERE salary > 10000
      AND department_id > 90;


EMPLOYEE_ID|LAST_NAME|FIRST_NAME|SALARY|DEPARTMENT_ID|
-----------+---------+----------+------+-------------+
        205|Higgins  |Shelley   | 12008|          110|
        108|Greenberg|Nancy     | 12008|          100|
        205|Higgins  |Shelley   | 12008|          110|

```

### 교집합(INTERSECT)
- 서로 다른 쿼리구문에서 동일한 결과의 행들만 선택하여 출력하는 명령어

```sql

SELECT employee_id,
       last_name,
       first_name,
       salary,
       department_id
FROM employees
WHERE salary > 10000
      AND department_id >= 100
INTERSECT
SELECT employee_id,
       last_name,
       first_name,
       salary,
       department_id
FROM employees
WHERE salary > 10000
      AND department_id >= 90;


EMPLOYEE_ID|LAST_NAME|FIRST_NAME|SALARY|DEPARTMENT_ID|
-----------+---------+----------+------+-------------+
        108|Greenberg|Nancy     | 12008|          100|
        205|Higgins  |Shelley   | 12008|          110|
```

### 차집합(MINUS)

```sql
SELECT employee_id,
       last_name,
       first_name,
       salary,
       department_id
FROM employees
WHERE salary > 10000
      AND department_id >= 90
MINUS
SELECT employee_id,
       last_name,
       first_name,
       salary,
       department_id
FROM employees
WHERE salary > 10000
      AND department_id >= 100;

EMPLOYEE_ID|LAST_NAME|FIRST_NAME|SALARY|DEPARTMENT_ID|
-----------+---------+----------+------+-------------+
        100|King     |Steven    | 24000|           90|
        101|Kochhar  |Neena     | 17000|           90|
        102|De Haan  |Lex       | 17000|           90|

```

## 계층형 쿼리
- start with	: 가장 상단에 위치할 행의 조건
- connect by	: 계층의 관계를 설정하는 조건
- prior		: connect by절에서 사용. 상위 계층에 해당하는 컬럼 옆에 작성

```sql
- 상위계층이 가지고 있는 manager_id컬럼의 값과 동일한 employee_id를 가진 행을 하위계층으로 연결하여 출력


SELECT employee_id, last_name, manager_id
FROM employees
START WITH employee_id = 107
CONNECT BY employee_id = PRIOR manager_id;


EMPLOYEE_ID|LAST_NAME|MANAGER_ID|
-----------+---------+----------+
        107|Lorentz  |       103|
        103|Hunold   |       102|
        102|De Haan  |       100|
        100|King     |          |

```

- 계층형 쿼리구문을 사용할 때 숨겨진 의사컬럼으로 LEVEL컬럼을 사용할 수 있음
- LEVEL 컬럼은 해당 행이 상위 몇 번째 계층인지를 나타내고 있는 값이며 숫자로 표현됨
- 최상위 계층은 LEVEL컬럼의 값이 1임

```sql

SELECT
	LEVEL,
	employee_id,
	lpad(' ', LEVEL, ' ')  --LPAD('출력문자', 출력을 원하는 길이(숫자), '여백문자')
       || first_name
       || ' '
       || last_name name,
	manager_id
FROM
	employees
START WITH
	manager_id IS NULL
CONNECT BY
	PRIOR employee_id = manager_id;


LEVEL|EMPLOYEE_ID|NAME                 |MANAGER_ID|
-----+-----------+---------------------+----------+
    1|        100| Steven King         |          |
    2|        101|  Neena Kochhar      |       100|
    3|        108|   Nancy Greenberg   |       101|
    4|        109|    Daniel Faviet    |       108|
    4|        110|    John Chen        |       108|
    4|        111|    Ismael Sciarra   |       108|
    4|        112|    Jose Manuel Urman|       108|
    4|        113|    Luis Popp        |       108|
    3|        200|   Jennifer Whalen   |       101|
    3|        203|   Susan Mavris      |       101|
    3|        204|   Hermann Baer      |       101|
    3|        205|   Shelley Higgins   |       101|
    4|        206|    William Gietz    |       205|
    2|        102|  Lex De Haan        |       100|
    3|        103|   Alexander Hunold  |       102|
    4|        104|    Bruce Ernst      |       103|
    4|        105|    David Austin     |       103|
    4|        106|    Valli Pataballa  |       103|
    4|        107|    Diana Lorentz    |       103|
    2|        114|  Den Raphaely       |       100|
    3|        115|   Alexander Khoo    |       114|
    3|        116|   Shelli Baida      |       114|
…
```
  



---

# [오후수업] JAVA 56차

## AdWidget_WebView

```xml
----------------------------------------AndroidManifest.xml----------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.and0613_adwidget_webview">

    <!-- 인터넷 사용 권한 부여 -->
    <uses-permission android:name="android.permission.INTERNET"/>

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.And0613_AdWidget_WebView"
        android:usesCleartextTraffic="true">
        <!-- HTTP 프로토콜 사용을 위해 usesCleartextTraffic="true" 속성 설정 필수! -->

        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
----------------------------------------activity_main.xml----------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <!--
        주소(URL) 입력을 위한 EditText
        => 한 줄로 모든 주소를 입력받기 위해 singleLine 속성 true 로 설정
        -->

            <EditText
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:id="@+id/etUrl"
                android:layout_weight="1"
                android:singleLine="true"/>

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="이동"
            android:id="@+id/btnGo"
            android:textSize="20sp"/>

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/btnBack"
            android:text="이전"
            android:textSize="20sp"/>


    </LinearLayout>

    <WebView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/webView"/>
</LinearLayout>
```

```java
package com.example.and0613_adwidget_webview;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.KeyEvent;
import android.view.View;
import android.webkit.WebSettings;
import android.webkit.WebView;
import android.webkit.WebViewClient;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    EditText etUrl;
    Button btnGo, btnBack;
    WebView webView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);


        etUrl = findViewById(R.id.etUrl);
        btnGo = findViewById(R.id.btnGo);
        btnBack = findViewById(R.id.btnBack);
        webView = findViewById(R.id.webView);

        // 입력된 URL에 접속하여 페이지 내용을 WebView 위젯에 표시
        // WebViewClient 서브클래스의 인스턴스 생성
        MyWebViewClient myWebViewClient = new MyWebViewClient();
        
        // WebView 객체의 setWebViewClient() 메서드를 호출하여
        // WebViewClient 서브클래스 인스턴스 전달
        webView.setWebViewClient(myWebViewClient);

        WebSettings webSettings = webView.getSettings();
        webSettings.setBuiltInZoomControls(true);   // 줌 기능(확대/축소 버튼) 설정
        webSettings.setJavaScriptEnabled(true);     // 자바스크립트 허용 설정

        
        btnGo.setOnClickListener(view -> goUrl());

        btnBack.setOnClickListener(view -> {
            webView.goBack();

        });
        
        // EditText에 이벤트 연결하여 엔터키 입력 시 이동 기능 동작 수행
        etUrl.setOnKeyListener(new View.OnKeyListener() {
            @Override
            public boolean onKey(View view, int keyCode, KeyEvent keyEvent) {
                if(keyCode == KeyEvent.KEYCODE_ENTER) {
                    goUrl();
                    return true;
                }
                return false;
            }
        });





    } // onCreate() 메서드 끝
    
    public void goUrl() {
        // EditText 에 입력된 URL 가져오기
        String url = etUrl.getText().toString();

        // 만약, URL 이 입력되지 않았을 경우 오류 메세지 출력 및 커서 요청
        if (url.length() == 0) { // url.equals("") 와 동일
            Toast.makeText(MainActivity.this, "URL 입력 필수!", Toast.LENGTH_SHORT).show();
            etUrl.requestFocus();
            return;
        }


        // WebView 객체의 LoadUrl() 메서드를 호출하여
        // EditText로 부터 가져온 URL 정보 전달

        webView.loadUrl(url);
    }

    
    
    
    // WebView 위젯 동작을 위해 webViewClient 클래스를 상속받는 서브클래스 정의 - 내부클래스
    class MyWebViewClient extends WebViewClient {
        // shouldOverrideUrlLoading() 메서드 오버라이딩
        @Override
        public boolean shouldOverrideUrlLoading(WebView view, String url) {
            return super.shouldOverrideUrlLoading(view, url);
        }
    }

}   // MainActivity 클래스 끝



```

## SnackBar
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/btnSnackBar"
        android:text="스낵바 표시하기"
        android:textSize="20sp"/>

</LinearLayout>
```
```java
package com.example.and0613_snackbar;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;

import com.google.android.material.snackbar.Snackbar;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button btnSnackBar = findViewById(R.id.btnSnackBar);
        btnSnackBar.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                // SnackBar 를 사용하려면 Toast 와 마찬가지로 static 메서드인
                // make() 메서드를 호출하여 표시
                // => 주의! Toast 클래스의 makeText() 메서드 첫 번째 파라미터가
                //    Context 객체이므로 현재 컨텍스트(액티비티) 객체의 this를 전달했지만,
                //    SnackBar 클래스의 make() 메서드 첫번째 파라미터는 View 타입 객체이므로
                //    onClick() 메서드의 파라미터로 전달된 View 타입 객체 v 전달하면 된다.
               Snackbar.make(view, "스낵바 메시지 입니다.", Snackbar.LENGTH_SHORT).show();
            }
        });
    }
}
```

## Toast
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="horizontal">

    <EditText
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:textSize="20sp"
        android:hint="X좌표"
        android:id="@+id/et1"/>

    <EditText
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:textSize="20sp"
        android:hint="Y좌표"
        android:id="@+id/et2"/>

    <Button
        android:id="@+id/btn"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:onClick="onButtonClick"
        android:scrollbarSize="20sp"
        android:text="입력완료" />
</LinearLayout>
```
```java
package com.example.and0613_toast;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.Gravity;
import android.view.View;
import android.widget.EditText;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    EditText et1, et2;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // EditText 아이디값 가져오기
        et1 = findViewById(R.id.et1);
        et2 = findViewById(R.id.et2);
    }

    // 레이아웃 내의 위젯에서 onClick 속성에 대한 이벤트 지정할 경우
    // ex) android:onClick="onButtonClick"
    //     => onButtonClick 이름과 동일한 메서드 정의 (파라미터는 무조건 View 타입 변수 1개 필수!)
    public void onButtonClick(View v) {
        int xOffset = Integer.parseInt(et1.getText().toString());
        int yOffset = Integer.parseInt(et2.getText().toString());

        // 1. Toast 객체 생성 (파라미터로 컨텍스트 객체 (this), 표시할 메세지, 표시 길이 지정)
        // => 컨텍스트 객체 : 표시할 액티비티(별도의 액티비티가 없으면 this 사용 가능)
        // => 표시할 메세지 : 토스트로 출력할 메세지 문자열
        // => 표시 길이 : 토스트 메세지의 출력 시간
        //               (Toast.LENGTH_LONG : 출력시간 김, Toast.LENGTH_SHORT : 출력시간 짧음)
        Toast toast = Toast.makeText(this, "위치가 바뀐 토스트 메시지", Toast.LENGTH_SHORT);

        
        // 2. Toast 객체의 각종 설정 수행
        // => ex) 출력 위치 변경을 위해 setGravity() 메서드 호출
        // => setGravity() 메서드 파라미터로 출력 위치를 Gravity.xxx 상수로 지정
        toast.setGravity(Gravity.TOP, xOffset, yOffset);
        // => API level 30 부터 지원되지 않는 기능이므로 사용 불가

        // 3. Toast 객체의 show() 메서드를 호출하여 토스트 메세지 출력
        toast.show();
    }
}
```
