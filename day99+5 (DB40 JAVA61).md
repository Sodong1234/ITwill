# [오전수업] DB 40차
## 데이터베이스 암호화
- 기존의 암호화 구현 방식은 클라이언트 쪽에서 암호화와 복호화를 거쳐 데이터를 처리하는 방식으로 프로그램에서 암호화를 가정하지 않고 개발한 상태에서 추가로 암호화를 도입한다면 추가적인 개발 비용과 클라이언트의 성능 저하가 발생함
- 데이터베이스에서 암호화를 지원하는 경우 데이터베이스의 내부 기능 중 암호화를 활성화하여 사용하게 되므로 추가적인 비용 발생은 없으며, 성능은 일부 저하가 발생하고, 알고리즘에 따라 저장공간의 사용량이 늘어나게 된다. 또한 클라이언트 부분에서는 별도의 구조변경이나 성능저하가 발생하지는 않음

![unnamed](https://user-images.githubusercontent.com/95197594/175223353-402a3551-8066-4f54-982a-7ae849b23203.png)

## TDE(Transparent Data Encrypion)
- 오라클 데이터베이스에서 지원하는 암호화 방식으로 키방식의 암호화를 사용할 수 있음
- 키는 데이터의 암호화와 복호화에 사용되는 값으로 공개키, 개인키가 있으며, 이런 값들은 전자지갑이라는 특정 저장공간 또는 하드웨어를 통하여 관리

### 전자지갑 생성하기
```sql
[oracle@itwillbs ~]$ cd $ORACLE_HOME/network/admin


- 전자지갑의 설정은 sqlnet.ora의 파일로 작성한다.
[oracle@itwillbs admin]$ vi sqlnet.ora


# 키보드 i 입력 후 아래의 내용 오탈자 없이 정확하게 입력!
ENCRYPTION_WALLET_LOCATION=
(SOURCE=
        (METHOD=FILE)
        (METHOD_DATA=
                (DIRECTORY=/u01/app/oracle/admin/orcl/wallet)))

# 입력 후 키보드 esc입력 후 :wq로 저장 후 종료

- 전자지갑이 생성 될 경로 디렉토리 생성
[oracle@itwillbs admin]$ cd $ORACLE_BASE/admin/$ORACLE_SID
[oracle@itwillbs orcl]$ mkdir wallet


- 이후 데이터베이스 시작 / sys계정으로 로그인 후 아래의 암호화 관리 문장 생성 구문 실행
- 아래의 구문에서 oracle_13은 임의의 문자열 값으로 암호 관리에 사용되는 문장
  다른 문장으로 작성해도 상관은 없으나 암호화 기능 사용 시 필요함.
SQL> ALTER SYSTEM SET ENCRYPTION KEY
  2  IDENTIFIED BY oracle_13;


- System altered.

sqlplus → 리눅스 터미널 환경
SQL> !


- 전자지갑 생성 여부 확인!
[oracle@itwillbs wallet]$ cd $ORACLE_BASE/admin/$ORACLE_SID/wallet
[oracle@itwillbs wallet]$ ls

ewallet.p12
```

## 컬럼레벨 암호화
- 암호화가 필요한 컬럼에 개별적으로 암호화를 적용하는 방법
- 컬럼레벨의 암호화의 기본 암호화 알고리즘은 AES192를 사용
```sql

SQL> conn hr/hr

Connected.

- 암호화 된 컬럼을 포함한 테이블 생성구문
SQL> CREATE TABLE new_table
  2  (first_name VARCHAR2(128),
  3  last_name VARCHAR2(128),
  4  empid NUMBER,
  5  salary NUMBER(6) ENCRYPT);


Table created.

- 테이블 구조 조회 시 암호화가 적용된 컬럼은 ENCRYPT 키워드가 붙어 출력 됨
SQL> DESC new_table

 Name                                      Null?    Type
 ----------------------------------------- -------- ----------------------------
 FIRST_NAME                                         VARCHAR2(128)
 LAST_NAME                                          VARCHAR2(128)
 EMPID                                              NUMBER
 SALARY                                             NUMBER(6) ENCRYPT

- 데이터 딕셔너리에서 암호화 된 컬럼의 상세 정보 조회
SELECT * FROM user_encrypted_columns;

TABLE_NAME|COLUMN_NAME|ENCRYPTION_ALG  |SALT|INTEGRITY_ALG|
----------+-----------+----------------+----+-------------+
NEW_TABLE |SALARY     |AES 192 bits key|YES |SHA-1        |


- 암호화 컬럼이 있는 테이블에 데이터 입력 테스트
SQL> INSERT INTO new_table
  2  VALUES ('&f_name', '&l_name', &e_id, &sal);


Enter value for f_name: Hi
Enter value for l_name: Lo
Enter value for e_id: 10
Enter value for sal: 2000
old   2: VALUES ('&f_name', '&l_name', &e_id, &sal)
new   2: VALUES ('Hi', 'Lo', 10, 2000)

- sqlplus에서 /는 이전 입력 구문 재실행 명령어
SQL> /

Enter value for f_name: c3
Enter value for l_name: tw
Enter value for e_id: 20
Enter value for sal: 3000
old   2: VALUES ('&f_name', '&l_name', &e_id, &sal)
new   2: VALUES ('c3', 'tw', 20, 3000)

1 row created.

SQL> /

Enter value for f_name: Ky
Enter value for l_name: pro
Enter value for e_id: 30
Enter value for sal: 1000
old   2: VALUES ('&f_name', '&l_name', &e_id, &sal)
new   2: VALUES ('Ky', 'pro', 30, 1000)

1 row created.

SQL> commit;


Commit complete.

- 현재 전자지갑이 열려 있어 마스터키로 암호화된 데이터를 복호화하여 사용할 수 있는 상태이므로 salary컬럼의 값도 문제 없이 사용 가능
SELECT * FROM new_table;

FIRST_NAME|LAST_NAME|EMPID|SALARY|
----------+---------+-----+------+
Hi        |Lo       |   10|  2000|
c3        |tw       |   20|  3000|
Ky        |pro      |   30|  1000|
```

### 기존 테이블에 암호화 적용된 컬럼 추가
```sql
SQL> ALTER TABLE new_table
  2  ADD (dept VARCHAR2(30) ENCRYPT);


Table altered.

SQL> DESC new_table

 Name                                      Null?    Type
 ----------------------------------------- -------- ----------------------------
 FIRST_NAME                                         VARCHAR2(128)
 LAST_NAME                                          VARCHAR2(128)
 EMPID                                              NUMBER
 SALARY                                             NUMBER(6) ENCRYPT
 DEPT                                               VARCHAR2(30) ENCRYPT
```

### 기존 테이블의 컬럼에 암호화 적용하기
```sql
SQL> ALTER TABLE new_table
  2  MODIFY (last_name ENCRYPT);


Table altered.

SQL> DESC new_table

 Name                                      Null?    Type
 ----------------------------------------- -------- ----------------------------
 FIRST_NAME                                         VARCHAR2(128)
 LAST_NAME                                          VARCHAR2(128) ENCRYPT
 EMPID                                              NUMBER
 SALARY                                             NUMBER(6) ENCRYPT
 DEPT                                               VARCHAR2(30) ENCRYPT
```

## 암호화 된 컬럼 복호화하기
```sql
SQL> ALTER TABLE new_table
  2  MODIFY (last_name DECRYPT);


Table altered.

SQL> DESC new_table

 Name                                      Null?    Type
 ----------------------------------------- -------- ----------------------------
 FIRST_NAME                                         VARCHAR2(128)
 LAST_NAME                                          VARCHAR2(128)
 EMPID                                              NUMBER
 SALARY                                             NUMBER(6) ENCRYPT
 DEPT                                               VARCHAR2(30) ENCRYPT
```

### 기본값이 아닌 암호화 알고리즘 사용하기
- USING을 사용하여 기본값이 아닌 암호화 알고리즘을 사용할 수 있다.

![unnamed (1)](https://user-images.githubusercontent.com/95197594/175225200-dc4505c2-0d53-444a-ab7e-4c5b834762dc.png)
```sql
SQL> CREATE TABLE new_table1
  2  (first_name VARCHAR2(128),
  3  emp_id NUMBER,
  4  salary NUMBER(6) ENCRYPT USING 'AES256');


Table created.

SELECT * FROM user_encrypted_columns;

TABLE_NAME|COLUMN_NAME|ENCRYPTION_ALG  |SALT|INTEGRITY_ALG|
----------+-----------+----------------+----+-------------+
NEW_TABLE |SALARY     |AES 192 bits key|YES |SHA-1        |
NEW_TABLE |DEPT       |AES 192 bits key|YES |SHA-1        |
NEW_TABLE1|SALARY     |AES 256 bits key|YES |SHA-1        |
```

### 테이블 암호화 알고리즘 변경하기
- 복호화 키는 테이블 단위로 만들어 관리하므로 하나의 테이블에는 한 종류의 암호화 알고리즘만 적용 할 수 있음
```sql

SQL> ALTER TABLE new_table1
  2  REKEY USING '3DES168';


Table altered.

SELECT * FROM user_encrypted_columns;

TABLE_NAME|COLUMN_NAME|ENCRYPTION_ALG               |SALT|INTEGRITY_ALG|
----------+-----------+-----------------------------+----+-------------+
NEW_TABLE |SALARY     |AES 192 bits key             |YES |SHA-1        |
NEW_TABLE |DEPT       |AES 192 bits key             |YES |SHA-1        |
NEW_TABLE1|SALARY     |3 Key Triple DES 168 bits key|YES |SHA-1        |
```

## 암호화 방식
### SALT 방식 (기본값)
- 암호화 적용시 임의의 쓰레기값을 더하여 암호화를 진행하는 방식으로 동일한 값을 암호화하더라도 그 결과는 다르게 만들어진다. 보안상 더 좋으나 이 경우 인덱스의 효과가 없어진다. 따라서 PK, UK 제약조건이 적용된 컬럼에는 사용 불가.

![unnamed (2)](https://user-images.githubusercontent.com/95197594/175225378-4783ae33-e57a-44e7-af30-c0fb007c5514.png)

### NOSALT 방식
- 별도 임의값을 더하지 않고 저장된 값 그대로 암호화를 적용하는 방식. 이 경우 동일값을 암호화한 결과는 동일한 값으로 출력값이 같은지 아닌지를 유추할 수 있기 때문에 보안상 취약해진다. 인덱스 사용 가능해짐.


---

# [오후수업] JAVA 61차
## Activity_Explict_Intent
```xml
--------------------------------------------------activity_main.xml--------------------------------------------------
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
        android:id="@+id/btnNewActivity"
        android:text="새 액티비티(화면)열기"
        android:textSize="20sp"
        android:onClick="showNewActivity"/>

</LinearLayout>
--------------------------------------------------second_activity.xml--------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#FFFF88">

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/btnReturn"
        android:text="돌아가기"
        android:textSize="20sp"/>

</LinearLayout>
```
```java
--------------------------------------------------MainActivity.java--------------------------------------------------
package com.example.and0623_activity_explict_intent;

import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.os.Bundle;
import android.view.View;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    public void showNewActivity(View v){
        Intent intent = new Intent(MainActivity.this, SecondActivity.class);

        // 명시적 인텐트 사용 시 새 액티비티에 데이터를 전달하는 방법
        // Intent 객체의 putExtra() 메서드를 호출하여 데이터의 이름과 값 지정
        // => Map 객체 사용법과 동일하며, 복수개의 데이터는 putExtra() 메서드 반복 사용
//        intent.putExtra("data1", 10);   // data1이라는 키값으로 정수데이터 10을 저장
//        intent.putExtra("data2", 20);   // data2이라는 키값으로 정수데이터 20을 저장

        String[] strNames = {"홍길동", "이순신", "강감찬"};
        intent.putExtra("strNames", strNames);

        startActivity(intent);
    }
}
--------------------------------------------------SecondActivity.java--------------------------------------------------
package com.example.and0623_activity_explict_intent;

import android.content.Intent;
import android.os.Bundle;
import android.widget.Button;
import android.widget.Toast;

import androidx.appcompat.app.AppCompatActivity;

import java.util.Arrays;

public class SecondActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.second_activity);

        // MainActivity 로 부터 전달받은 명시적 인텐트 내의 데이터 가져오는 방법
        // Intent 객체를 가져오기 위해 getIntent() 메서드를 호출하고
        // 가져온 Intent 객체의 getXXXExtra() 메서드를 호출하여 전달된 데이터의 키를 지정
        // => 이 때, getXXXExtra() 메서드의 XXX은 전달된 데이터의 자바 데이터타입 이름
        Intent intent = getIntent();
//        int num1 = intent.getIntExtra("data1", 0);
//        int num2 = intent.getIntExtra("data2", 0);
//
//        Toast.makeText(this, num1 + ", " + num2, Toast.LENGTH_SHORT).show();


        // String[] 타입으로 전달된 데이터를 Intent 객체로부터 가져와서
        // 배열에있는 모든 데이터를 출력
        String[] strNames = intent.getStringArrayExtra("strNames");

//        String str = "";
//        for(String s : strNames){
//            str += s + "/";
//        }

        String str = Arrays.toString(strNames);

        Toast.makeText(this, str, Toast.LENGTH_SHORT).show();

        Button btnReturn = findViewById(R.id.btnReturn);
        btnReturn.setOnClickListener(view -> finish());
    }
}

```
## Activity_Explict_Intent_TwoWay
```xml
--------------------------------------------------activity_main.xml--------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical"
    android:padding="10dp">

    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/etNum1"
        android:textSize="30sp"
        android:hint="숫자1 입력"/>

    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/etNum2"
        android:textSize="30sp"
        android:hint="숫자2 입력"/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/btnAdd"
        android:text="더하기"
        android:textSize="30sp"/>

</LinearLayout>

--------------------------------------------------activity_second.xml--------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/btnReturn"
        android:text="돌아가기"
        android:textSize="30sp"/>

</LinearLayout>
```
```java
--------------------------------------------------MainActivity.java--------------------------------------------------
package com.example.and0623_activity_explict_intent_twoway;

import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.os.Bundle;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        EditText etNum1 = findViewById(R.id.etNum1);
        EditText etNum2 = findViewById(R.id.etNum2);
        Button btnAdd = findViewById(R.id.btnAdd);

        // 더하기 버튼 클릭 시 새 액티비티(SecondActivity)로 전환
        // => 이때, 명시적 인텐트를 사용하여, 입력받은 정수 2개를 포함하여 전달
        // => 또한, 계산 결과를 리턴받기위해 startActivity() 메서드가 아닌
        //    startActivityForResult() 메서드를 호출하여 새 액티비티로 전환
        btnAdd.setOnClickListener(view -> {

            int num1 = Integer.parseInt(etNum1.getText().toString());
            int num2 = Integer.parseInt(etNum2.getText().toString());

            Intent intent = new Intent(MainActivity.this, SecondActivity.class);

            intent.putExtra("num1", num1);
            intent.putExtra("num2", num2);

//            startActivity(intent);
            // startActivityForResult(인텐트객체, 요청코드)
            // => 요청코드(requestCode)는 여러 액티비티로부터 응답이 리턴되어야하는 경우
            //    각 응답 액티비티를 구분할 목적으로 사용함(구분이 필요없으면 0이상 아무거나)
            startActivityForResult(intent, 0);
        });

    } // onCreate() 메서드 끝

    // 다른 액티비티에서 응답을 전달받기 위해 startActivityForResult() 메서드를 호출한 경우
    // 해당 액티비티로부터 finish() 메서드에 의해 다시 현재 액티비티로 돌아올때
    // onActivity() 메서드가 자동으로 호출되므로 오버라이딩 필수!

    @Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        super.onActivityResult(requestCode, resultCode, data);

        // 만약, 여러 액티비티로부터 응답이 돌아오는 경우
//        if(requestCode == 0) {  // SecondActivity
//
//        } else if (requestCode == 1) { // ThirdActivity
//
//        }

        // resultCode 가 RESULT_OK 값이면 정상 응답이므로 응답 데이터 처리
        if(resultCode == RESULT_OK) {
            // 응답 Intent 객체는 이미 파라미터로 전달되었으므로 getIntent() 불필요
            // 응답 데이터 바로 꺼낼 수 있음
            int result = data.getIntExtra("result", 0);
            Toast.makeText(this, "덧셈 결과 : " + result, Toast.LENGTH_SHORT).show();

            finish();
        }



    }
}
--------------------------------------------------SecondActivity.java--------------------------------------------------
package com.example.and0623_activity_explict_intent_twoway;

import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;

public class SecondActivity extends AppCompatActivity {

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_second);

        Intent intent = getIntent();
        int num1 = intent.getIntExtra("num1", 0);
        int num2 = intent.getIntExtra("num2", 0);

//        Toast.makeText(this, num1 + ", " + num2, Toast.LENGTH_SHORT).show();
        int result = num1 + num2;

        Button btnReturn = findViewById(R.id.btnReturn);
        // 돌아가기 버튼 클릭 시 현재 액티비티 종료
        // => 단, 정수 2개의 덧셈 결과를 새 Intent 객체에 저장하여 리턴
//        btnReturn.setOnClickListener(new View.OnClickListener() {
//            @Override
//            public void onClick(View view) {
//
//                // Intent 객체 생성
//                // => MainActivity 에서 이동할 때 설정한 정보와 반대로 설정 필요
//                Intent returnIntent = new Intent(SecondActivity.this, MainActivity.class);
//
//                returnIntent.putExtra("result", result);
//
//                // 현재 액티비티에서 리턴할 Intent 객체를 전달하기 위해서는
//                // setResult() 메서드를 호출하여 응답코드(ResultCode)와 인텐트 객체 전달
//                setResult(RESULT_OK, returnIntent);
//
//                finish();
//            }
//        });

        // -------------------------------------------------------------------------
        // 만약, 계산 즉시 현재 액티비티를 종료하고 돌아가려면
        // 버튼 이벤트 없이 바로 응답 작업 구현하면 된다!
        Intent returnIntent = new Intent(SecondActivity.this, MainActivity.class);
        returnIntent.putExtra("result", result);
        setResult(RESULT_OK, returnIntent);
        finish();
    }
}

```

