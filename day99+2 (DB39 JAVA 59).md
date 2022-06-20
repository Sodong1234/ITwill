# [오전수업] DB 39차

## with admin option
- system 권한에서 사용되는 옵션 절로 해당 옵션절을 사용한 grant 구문을 실행하는 경우 단순한 실행권한 뿐 아니라 해당 권한을 부여, 회수 할 수 있는 관리 권한을 얻게 됨
- 권한을 부여한 계정이 권한을 회수 당하더라도 시스템 권한은 종속적으로 회수되지 않음

![image](https://user-images.githubusercontent.com/95197594/174510812-b6f36b9c-ac33-432a-9a6c-8508975c82ea.png)

* 실습 사용 계정 생성
```sql
SQL> conn sys/oracle as sysdba

Connected.
SQL> CREATE USER turner
  2  IDENTIFIED BY lover;


User created.

SQL> CREATE USER ford
  2  IDENTIFIED BY henry;


User created.

SQL> GRANT create session, unlimited tablespace
  2  TO turner, ford;


Grant succeeded.
```

* WITE ADMIN OPTION 실습
```sql
- SYS → turner으로 create table 관리 권한 부여
SQL> GRANT create table
  2  TO turner
  3  WITH ADMIN OPTION;


Grant succeeded.

SQL> conn turner/lover

Connected.
- 테이블 생성 권한 확인
SQL> CREATE TABLE turner_table (id NUMBER);


Table created.

- turner → ford create table 권한 부여
SQL> GRANT create table
  2  TO ford;


Grant succeeded.

SQL> conn ford/henry

Connected.
- ford 계정에서도 테이블 생성 확인
SQL> CREATE TABLE ford_table (id NUMBER);


Table created.

- sys ← turner 권한 회수
SQL> conn sys/oracle as sysdba

Connected.
SQL> REVOKE create table
  2  FROM turner;


Revoke succeeded.

- 기존 turner에게 create table 권한을 부여받은 ford로 create table 권한 유지 확인
SQL> conn ford/henry

Connected.
SQL> CREATE TABLE ford_table2 ( id NUMBER);


Table created.
```

## WITH GRANT OPTION
- 오브젝트 권한에 대한 관리 권한을 부여하는 옵션절
- 관리 권한을 부여하더라도 소유권이 바뀌는 것은 아니므로 스키마명이 필요한 경우 오브젝트앞에 작성

![image](https://user-images.githubusercontent.com/95197594/174510947-677ebb4e-a4d9-413a-ab7a-3d8ce1058ce6.png)

## WITH GRANT OPTION 실습
```sql
- hr → turner 에게 employees 테이블에 대한 select권한을 관리 권한과 함께 부여
SQL> conn hr/hr

Connected.
SQL> GRANT select
  2  ON employees
  3  TO turner
  4  WITH GRANT OPTION;


Grant succeeded.

- turner로 hr.employees 테이블에 대한 select 권한 확인
SQL> conn turner/lover

Connected.
SQL> SELECT last_name, job_id
  2  FROM hr.employees
  3  WHERE employee_id = 103;


LAST_NAME                 JOB_ID
------------------------- ----------
Hunold                    IT_PROG

- turner → ford 로 hr.employees 테이블의 select 권한을 부여
SQL> GRANT select
  2  ON hr.employees
  3  TO ford;


Grant succeeded.

- ford도 hr.employees에 대한 select 권한을 확인
SQL> conn ford/henry

Connected.
SQL> SELECT last_name, job_id
  2  FROM hr.employees
  3  WHERE employee_id = 103;


LAST_NAME                 JOB_ID
------------------------- ----------
Hunold                    IT_PROG

- hr ← turner로 권한 회수
SQL> conn hr/hr

Connected.
SQL> REVOKE select
  2  ON employees
  3  FROM turner;


Revoke succeeded.

- ford는 hr에게 직접 hr.employees에 대한 select권한을 회수당했지는 않지만 turner계정이 권한을 회수당하며 종속적으로 권한을 회수당하였음.
SQL> conn ford/henry

Connected.
SQL> SELECT last_name, job_id
  2  FROM hr.employees
  3  WHERE employee_id = 103;

FROM hr.employees
        *
ERROR at line 2:
ORA-00942: table or view does not exist
```



---

# [오후수업] JAVA 59차

## AutoColpleteTextView
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical"
    android:padding="10dp">

   <AutoCompleteTextView
       android:layout_width="match_parent"
       android:layout_height="wrap_content"
       android:id="@+id/autoCompleteTextView"
       android:textSize="20sp"
       android:hint="자동완성텍스트뷰"
       android:completionHint="적합한 항목을 선택하세요."
       android:completionThreshold="3"/>

   <MultiAutoCompleteTextView
       android:layout_width="match_parent"
       android:layout_height="wrap_content"
       android:id="@+id/multiAutoCompleteTextView"
       android:textSize="20sp"
       android:hint="멀티자동완성텍스트뷰"
       android:completionHint="적합한 항목을 선택하세요."
       android:completionThreshold="2"/>

   <Button
       android:layout_width="match_parent"
       android:layout_height="wrap_content"
       android:id="@+id/btn"
       android:text="멀티자동완성텍스트뷰 입력 확인"
       android:textSize="20sp"/>
</LinearLayout>
```
```java
package com.example.and0620_autocolpletetextview;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.widget.ArrayAdapter;
import android.widget.AutoCompleteTextView;
import android.widget.Button;
import android.widget.MultiAutoCompleteTextView;
import android.widget.Toast;

import java.util.Arrays;
import java.util.List;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        AutoCompleteTextView autoCompleteTextView = findViewById(R.id.autoCompleteTextView);
        MultiAutoCompleteTextView multiAutoCompleteTextView = findViewById(R.id.multiAutoCompleteTextView);

        // 자동완성에 사용될 후보 단어 목록을 저장할 String[] 배열 생성
        String[] items = {
          "Java", "Javascript", "JSP Model 1", "JSP Model2",
          "Android", "HTML", "HTTP 기초", "Apache Server",
          "자바", "자바스크립트"
        };

        // AutoCompleteTextView : 자동완성 기준에 적합한 갯수의 글자가 입력되면
        //                        관련된 단어가 있을 경우 목록으로 표시하고
        //                        해당 목록의 단어를 선택하면 자동으로 입력하는 위젯
        // 후보 단어 목록을 ArrayAdapter 객체를 사용하여 생성
        // => 단어로 사용될 데이터가 String 타입이므로 제네릭 타입 String 지정
        // => 파라미터 : Context 객체, 목록을 표시할 형태(레이아웃), 목록으로 사용될 단어가 저장된 객체(빼열 또는 list)
        //    => 목록 표시 형태는 안드로이드에서 제공하는 레이아웃을 로딩하여 사용
        //       (android.R.layout.XXX 형태의 상수로 제공됨 => 주의! R.layout이 아님!)
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(this, android.R.layout.simple_dropdown_item_1line, items);

        autoCompleteTextView.setAdapter(adapter);
        // =======================================================================================
        // MultiAutoCompleteTextView : AutoCompleteTextView 와 기본 동작은 동일하지만
        // 하나의 단어만 자동완성 하는 것이 아니라, 단어 완성 후 콤마(,)를 붙여서
        // 다음 단어도 자동완성으로 동작하도록 하는 위젯
        // => 기본적인 설정은 거의 유사하며, AutoCompleteTextView와 ArrayAdapter 사용법 동일함
        List<String> items2 = Arrays.asList(items);   // 배열 -> list 객체로 변환
        ArrayAdapter<String> adapter2 = new ArrayAdapter(this, android.R.layout.simple_dropdown_item_1line, items2);
        multiAutoCompleteTextView.setAdapter(adapter2);

        // 자동완성 단어를 콤마(,)로 구분하기 위해 CommaTokenizer 객체 생성
        // => AutoCompleteTextView 와 달라지는 부분
        MultiAutoCompleteTextView.CommaTokenizer tokenizer = new MultiAutoCompleteTextView.CommaTokenizer();
        multiAutoCompleteTextView.setTokenizer(tokenizer);

        Button btn = findViewById(R.id.btn);
        btn.setOnClickListener(view -> {
            String str = multiAutoCompleteTextView.getText().toString();
            Toast.makeText(MainActivity.this, str, Toast.LENGTH_SHORT).show();
        });
    }
}
```

## Menu_OptionMenu

> - res 폴더에 New -> New Resource Directory에서 Resource type를 menu로 선택 후 생성
> - 생성한 menu 폴더에 menu.xml 생성
```xml
-----------------------------------------------------------------activity_main.xml-----------------------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical"
    android:gravity="center"
    android:id="@+id/baseLayout">

    <ImageView
        android:layout_width="150dp"
        android:layout_height="150dp"
        android:src="@drawable/ic_launcher"
        android:id="@+id/iv"/>

</LinearLayout>
-----------------------------------------------------------------menu.xml-----------------------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">

    <!--
    메뉴 생성을 위해서는
    1. menu 태그를 사용하여 메뉴 항목을 포함하는 툴 정의
    2. item 태그를 사용하여 메뉴의 각 항목 생성
        - id 속성을 통해 메뉴 항목 구분 가능
        - title 속성을 통해 메뉴의 텍스트 설정
    -->
    <item
        android:id="@+id/itemRed"
        android:title="배경색(빨강)"/>
    <item
        android:id="@+id/itemGreen"
        android:title="배경색(초록)"/>
    <item
        android:id="@+id/itemBlue"
        android:title="배경색(파랑)"/>

    <!--
     서브 메뉴 생성을 위해서는 item 테크 내에 다시 menu 태그를 사용하여
     서브 메뉴로 사용할 메뉴의 항목들을 생성
     -->

    <item
        android:title="버튼 변경(서브메뉴)">

        <menu>

            <item
                android:id="@+id/subItemRotate"
                android:title="이미지 회전"/>
            <item
                android:id="@+id/subItemExpand"
                android:title="이미지 확대"/>
        </menu>

    </item>





</menu>
```
```java
package com.example.and0620_menu_optionmenu;

import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;

import android.graphics.Color;
import android.os.Bundle;
import android.view.Menu;
import android.view.MenuInflater;
import android.view.MenuItem;
import android.view.SubMenu;
import android.widget.ImageView;
import android.widget.LinearLayout;

public class MainActivity extends AppCompatActivity {

    LinearLayout baseLayout;
    ImageView iv;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        baseLayout = findViewById(R.id.baseLayout);
        iv = findViewById(R.id.iv);
    }

    // 옵션메뉴를 등록하기 위해서는 onCreateOptionMenu() 메서드 오버라이딩 필수!
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        super.onCreateOptionsMenu(menu);

        // XML로 옵션메뉴 생성하기

        // 메뉴 XML 파일을 메모리에 로딩하기 위해서는 MenuInflater 객체 사용 필요
        // => getMenuInflater() 메서드를 호출하여 MenuInflater 객체 가져온 후
        //    inflate() 메서드를 호출하여 로딩할 XML 파일과 Menu 객체 전달
//        MenuInflater menuInflater = getMenuInflater();
//        menuInflater.inflate(R.menu.menu, menu);
        // ======================================================

        // 자바코드로 옵션메뉴 생성하기

        // onCreateOptionMenu() 메서드 파라미터로 전달되는 Menu 객체의
        // add() 메서드를 호출하여 각 메뉴 항목 추가
        menu.add(0, 1, 0, "배경색(빨강)");
        menu.add(0, 2, 0, "배경색(초록)");
        menu.add(0, 3, 0, "배경색(파랑)");
//
//        // 서브메뉴 생성시에는 SubMenu 객체를 사용
//        // => Menu 객체의 addSubMenu() 메서드를 호출하여 SubMenu 객체 생성하고
//        //    SubMenu 객체의 add() 메서드를 호출하여 각 서브메뉴 항목 추가
        SubMenu sMenu = menu.addSubMenu("버튼 변경(서브메뉴) >>");
        sMenu.add(0, 4, 0, "이미지 회전");
        sMenu.add(0, 5, 0, "이미지 확대");

        return true;
    }

    // 옵션메뉴의 항목 클릭 시 동작 처리를 위헤서는 onOptionItemSelected() 메서드 오버라이딩 필수


    @Override
    public boolean onOptionsItemSelected(@NonNull MenuItem item) {
        super.onOptionsItemSelected(item);

        // XML로 생성한 메뉴 이벤트 연결

//        switch (item.getItemId()) {
//            case R.id.itemRed:
//                baseLayout.setBackgroundColor(Color.RED);
//                break;
//            case R.id.itemGreen:
//                baseLayout.setBackgroundColor(Color.GREEN);
//                break;
//            case R.id.itemBlue:
//                baseLayout.setBackgroundColor(Color.BLUE);
//                break;
//            case R.id.subItemRotate:
//                iv.setRotation(iv.getRotation() + 30);
//                break;
//            case R.id.subItemExpand:
//                iv.setScaleX(2);
//                iv.setScaleY(2);
//                break;
//
//        }

        // 자바코드로 생성한 메뉴 이벤트 연결
        // case문의 값을 itemId 정수값 (1, 2, 3 ...) 사용
        switch (item.getItemId()) {
            case 1: // 배경색(빨강)
                baseLayout.setBackgroundColor(Color.RED);
                break;
            case 2: // 배경색(초록)
                baseLayout.setBackgroundColor(Color.GREEN);
                break;
            case 3: // 배경색(파랑)
                baseLayout.setBackgroundColor(Color.BLUE);
                break;
            case 4: // 서브메뉴의 이미지 회전
                iv.setRotation(iv.getRotation() + 30);
                break;
            case 5: // 서브메뉴의 이미지 확대
                iv.setScaleY(iv.getScaleY() + 2);
                iv.setScaleX(iv.getScaleX() + 2);
                break;
        }

        return true;
    }
}
```

