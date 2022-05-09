# [오전수업] JAVA 42차

## 버튼 생성

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical"
    android:background="#FFAA00">
    <!--
    XML 기본 속성
    1. Layout_width, Layout_height 속성: 위젯 등의 가로, 세로 크기 지정
       => match_parent : 위젯을 위젯이 위치한 부모(레이아웃 등)의 크기만큼 맞춤(꽉 채움)
       => wrap_content : 위젯을 위젯 내부의 내용(텍스트, 그림 등)에 맞춤
    2. id 속성 : 자바 코드에서 위젯을 연결하기 위해 필요한 아이디 지정
                => findViewById() 메서드를 호추하여 R.id.아이디 형태로 파라미터에 전달
    3. text 속성 : 텍스트뷰, 버튼, 라디오버튼 등에 표시할 텍스트 지정
    4. background : 해당 위젯의 배경색 설정
       => 주의! 안드로이드 스튜디오 4.1.x 부터 테마 적용으로 인해
          버튼은 backgroundTint 사용 필요
    -->

    <TextView
        android:id="@+id/tv"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="텍스트뷰 입니다."
        android:background="#FF0000"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/btn"
        android:text="버튼입니다."
        android:backgroundTint="#FF0000"/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="버튼입니다."/>

    <Button
        android:layout_width="100dp"
        android:layout_height="200dp"
        android:text="버튼입니다."/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:text="버튼입니다."/>

</LinearLayout>
```

---
## 텍스트
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="textSize 속성"
        android:textSize="30sp"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="textColor 속성"
        android:textColor="#0000FF"
        android:textSize="30sp"
        android:textStyle="normal|bold" />

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="textStyle 속성"
        android:textSize="30sp"
        android:textStyle="bold|italic" />

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="typeface 속성"
        android:textSize="30sp"
        android:typeface="serif" />

    <!--
    singleLine 속성
    => 텍스트를 표시할 때 한 줄 범위 내에서 표시할지 여부 결정
    true : 모든 텍스트를 한 줄 내에서 표시하고 모자랄 경우 생략하거나(TextView)
           가로 방향으로 스크롤되어 표시됨 (EditText)
    false : 텍스트 표시 가능 길이를 넘어설 경우 다음 줄로 넘김(줄바꿈 수행됨)
           => 다른 위젯등이 아래 쪽에 있을 경우 이동함

    -->
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="singleLine 속성singleLine 속성singleLine 속성singleLine 속성"
        android:textSize="30sp"
        android:singleLine="true"
        android:layout_marginBottom="30dp"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="TextView 연습1"
        android:textSize="30sp"
        android:id="@+id/tv1"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="TextView 연습2"
        android:textSize="30sp"
        android:id="@+id/tv2"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="TextView 연습3"
        android:textSize="30sp"
        android:id="@+id/tv3"/>
</LinearLayout>
```
```java
package com.example.and0509_basicwidget_textview;

import androidx.appcompat.app.AppCompatActivity;

import android.graphics.Color;
import android.graphics.Typeface;
import android.os.Bundle;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // TextView 타입 변수 3개 선언 및 TextView 위젯 ID를 사용하여 객체 연결
        TextView tv1 = findViewById(R.id.tv1);
        TextView tv2 = findViewById(R.id.tv2);
        TextView tv3 = findViewById(R.id.tv3);

        // 자바 코드를 사용하여 위젯 속성 변경
        // => ID를 통해 연결된 객체의 메서드를 호출하여 속성 변경 가능 (setXXX() 메서드 사용)

        // 1. 텍스트 색상 변경(textColor 속성)
//        tv1.setTextColor(Color.RED);    // Color.상수 활용 가능
//        tv1.setTextColor(Color.rgb(0, 255, 0));   // Color.rgb() 메서드에 RGB값 정수 사용 가능 (10진수)
        tv1.setTextColor(Color.rgb(0xff,0x00,0x00));    // 16진수 정수 사용 가능

        // 2. 텍스트 내용 변경(text 속성)
        tv1.setText("Hello, world!");

        // 3. 텍스트 스타일 변경(textStyle 속성)
        // => 주의! setTextStyle() 메서드는 존재하지 않으며 typeface 변경 시 함께 변경하므로
        //    setTypeface() 메서드 사용
        tv2.setTypeface(Typeface.SANS_SERIF, Typeface.BOLD_ITALIC);

        // 4. singleLine 속성 변경

        tv3.setText("singleLine 속성 변경singleLine 속성 변경singleLine 속성 변경singleLine 속성 변경");
        tv3.setSingleLine();
//        tv3.setSingleLine(true);
    }
}
```

---

## EditText 위젯
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical">
    <!--
    EditText 위젯 : 텍스트를 입력받을 수 있는 위젯
        =>  기본적으로 텍스트 입력 범위를 벗어나면 줄바꿈이 수행되므로
            singleLine 속성을 true로 설정하여 자동으로 옆으로 스크롤 되도록 변경 해야함
    -->
    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="30sp"
        android:layout_margin="10dp"
        android:singleLine="true"
        android:hint="텍스트를 입력하세요."
        android:id="@+id/edit"/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/btnOk"
        android:textSize="30sp"
        android:text="확인"
        android:layout_margin="10dp"/>

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/tv"
        android:text="입력받은 텍스트 출력 공간"
        android:textSize="20sp"
        android:layout_margin="10dp"/>

</LinearLayout>
```
```java
package com.example.and0509_basicwidget_edittext;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.KeyEvent;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // EditText와 Button 위젯 ID 가져와서 연결
        EditText edit = findViewById(R.id.edit);
        Button btnOk = findViewById(R.id.btnOk);
        TextView tv = findViewById(R.id.tv);

        // Button 객체에 OnclickListener 를 연결하여 버튼 위젯 클릭 이벤트 처리하도록 구현
        // => Button 객체의 setOnClickListener() 메서드를 호출하여
        //    OnClickListener 인터페이스 구현체를 파라미터로 전달
//        btnOk.setOnClickListener(new View.OnClickListener() {
//            @Override
//            public void onClick(View view) {
//                // EditText 위젯에서 입력받은 텍스트를 가져와서 출력
////                String str = edit.getText();    // 오류! 리턴타입이 Editable 이므로 저장 불가
//                String str = edit.getText().toString(); // toString() 메서드 호출하여 형변환 필수!
//
//                // 1. Toast 메세지로 출력할 경우
//                Toast.makeText(MainActivity.this, str, Toast.LENGTH_SHORT).show();
//
//                // 2. TextView 위젯에 출력할 경우
//                tv.setText(str);
//            }
//        });

        // 람다식???
        btnOk.setOnClickListener(view -> tv.setText(edit.getText().toString()));

        // EditText 위젯에 키보드 입력을 감지하는 리스너 연결
//        edit.setOnKeyListener(new View.OnKeyListener() {
//            @Override
//            public boolean onKey(View view, int keyCode, KeyEvent keyEvent) {
//
//                // 주의! Toast 에서 출력할 메세지는 파라미터 타입이 문자열 이므로
//                // 정수 데이터를 전달할 경우 문법 오류가 발생하지 않고, 실행 시 논리적 오류 발생함
//                // => 반드시 정수 -> 문자열로 변환하여 전달해야함!ㅏ
////                Toast.makeText(MainActivity.this, keyCode + "", Toast.LENGTH_SHORT).show();
//
//                // onKey() 메서드에 전달된 keyCode 값을 비교하여 입력받은 키 판별 가능
//                // => KeyEvent.KEYCODE_XXX 상수를 사용하여 원하는 키 지정
//                if(keyCode == keyEvent.KEYCODE_ENTER) { // 엔터키 감지
//                    // 엔터키 입력되면 EditText에 입력된 텍스트를 TextView 위젯에 출력
//                    String str = edit.getText().toString();
//                    tv.setText(str);
//                }
//
//                return false;
//            }
//        });

        // 람다식???
        edit.setOnKeyListener((v, keyCode, event) -> {

            if(keyCode == KeyEvent.KEYCODE_ENTER) {
                String str = edit.getText().toString();
                tv.setText(str);
            }

            return false;
        });
    }
}
```
---

## 여러가지 속성

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical"
    android:padding="20dp">

    <!--
    padding 속성 : 자신의 내부 위젯 또는 텍스트 등과 자신(레이아웃 또는 위젯 등)의
                  간격 조정 => 자신으로부터 대상을 얼마만큼 떨어뜨릴지 결정
    layout_margin 속성 : 자신과 상대방 사이의 간격 조정(위젯과 위젯 사이이 간격 등)
    -->

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="30sp"
        android:text="아래에 이름을 입력 : "
        android:padding="10dp"
        android:layout_margin="20dp"/>

    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="여기에 채우세요"
        android:textSize="30sp"
        android:layout_margin="20dp"/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="확인"
        android:textSize="30sp"
        android:layout_margin="20dp"/>

    <!--
    visibility 속성 : 해당 위젯 표시 여부 설정
    1) visible : 위젯 표시
    2) invisible : 위젯 미표시 (해당 위젯의 자리는 그대로 유지 = 단순 숨김처리)
    3) gone : 위젯 미표시 (해당 위젯 자리까지 전부 제거)

    enabled 속성 : 해당 위젯 활성화 여부 설정
          => false : 비활성화, 동작 중지, 색상 회색으로 변경됨
    clickable 속성 : 해당 위젯 클릭 가능 여부 설정
          => false : 클릭 불가
    -->

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="버튼1"/>


    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:visibility="invisible"
        android:text="버튼2" />

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="버튼3"/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="버튼4"
        android:id="@+id/btn4"
        android:enabled="false"/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="버튼5"
        android:id="@+id/btn5"
        android:clickable="false"/>

</LinearLayout>
```
```java
package com.example.and0509_basicwidget_padding_margin;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button btn4 = findViewById(R.id.btn4);
        Button btn5 = findViewById(R.id.btn5);

        btn4.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Toast.makeText(MainActivity.this, "버튼 4 클릭!", Toast.LENGTH_SHORT).show();
            }
        });

        btn5.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Toast.makeText(MainActivity.this, "버튼 5 클릭!", Toast.LENGTH_SHORT).show();

            }
        });
    }
}
```
---
---

# [오후수업] JSP 61차
> - http://finlife.fss.or.kr/ 에서 인증키 발급
> - http://finlife.fss.or.kr/PageLink.do?link=openapi/detail&menuId=2000118 API 활용

```jsp
------------------------------------test9_json.jsp------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="../js/jquery-3.6.0.js"></script>
<script type="text/javascript">
	$(function() {
		$("#btnOk").on("click", function() {
			$.ajax({
				type: "GET",
				url: "json_data9.txt",
				dataType: "json"
			})
			.done(function(data) {
				$("#resultArea").html("<table border='1'><tr><th colspan='3'>은행정보</th></tr></table>");
				$("#resultArea > table").append(
						"<tr><th width='50'>금융회사명</th>" 
						+ "<th width='500'>홈페이지주소</th>" 
						+ "<th width='100'>콜센터전화번호</th></tr>");
				
				let result = data.result;
				
				let baseList = result.baseList;
				
				
				for(let inf of baseList) {
					
					let kcn = baseList.kor_co_nm;
					let url = baseList.homp_url;
					let tel = baseList.cal_tel;
					
					$("#resultArea > table").append(
							"<tr><td>" + kcn + "</td>" 
							+ "<td>" + url + "</td>"
							+ "<td>" + tel + "</td></tr>"
					);
				}
			})
			.fail(function() {
				$("#resultArea").html("요청 실패");			
			});
		});
	});
		
		

</script>
</head>
<body>
	<h1>test7_json.jsp - 은행정보</h1>
	<input type="button" value="정보 조회" id="btnOk">
	<br>
	<div id="resultArea"></div>
</body>
</html>
------------------------------------json_data9.txt------------------------------------
* 위 주소에서 가져온 API를 txt 파일에 담음
```

