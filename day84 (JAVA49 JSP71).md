# [오전수업] JAVA 49차
## TableLayout
```xml
---------------------------------activity_main.xml----------------------------------
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

  <!--
   TableLayout : 위젯을 테이블(표) 형태로 배치하는 레이아웃
   - layout_width, layout_height 속성을 사용하여 가로, 세로 크기를 지정
     => 별도로 행 또는 열의 크기는 설정 생략 가능함
        (width, height 속성 사용 시 해당 위젯 크기만 변경되는 것이 아니라
         열 또는 행 크기 중 열 크기는 연동되어 변경됨)
   - stretchColumns 속성을 사용하여 남은 공간을 할당받을 열 지정 가능
     => 복수개의 열을 콤마로 구분하여 지정하면 해당 열이 모두 공간 할당받음
   - collapseColumns 속성으로 숨길 열 지정
     => 단, stretchColumns 에 같은 열이 지정되어 있으면
        해당되는 열은 보이지는 않지만 영역은 차지함
   -->

    <TableLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:stretchColumns="1,2"
        android:collapseColumns="0">

        <TableRow>

            <Button
                android:text="버튼1"
                android:textSize="20sp"/>

            <Button
                android:text="버튼2"
                android:textSize="20sp"/>

            <Button
                android:text="버튼3"
                android:textSize="20sp"/>

        </TableRow>

        <TableRow>

            <Button
                android:text="버튼4"
                android:textSize="20sp"/>

            <Button
                android:text="버튼5"
                android:textSize="20sp"/>

            <Button
                android:layout_height="100dp"
                android:text="버튼6"
                android:textSize="20sp"/>

        </TableRow>

        <TableRow>
            <!-- 테이블 1개 행 내의 열은 각각의 위젯을 사용하여 생성 -->

            <Button
                android:text="버튼7"
                android:textSize="20sp"
                android:layout_span="2"/>
            <!-- layout_span 속성을 사용하면 열을 합칠 수 있다! -->

            <Button
                android:text="버튼8"
                android:textSize="20sp"/>

        </TableRow>

        <TableRow>

            <Button
                android:text="버튼9"
                android:textSize="20sp"
                android:layout_column="1"/>
            <!-- layout_column 속성 사용 시 표시할 컬럼 설정 가능 -->

            <Button
                android:text="버튼10"
                android:textSize="20sp"/>

        </TableRow>

    </TableLayout>


</LinearLayout>
```

- 연습) 계산기 만들기

```xml
---------------------------------calculator.xml----------------------------------
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="10dp">

    <TableLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:stretchColumns="1,2,3,4,5">

        <TableRow>

            <EditText
                android:hint="숫자 1 입력"
                android:textSize="30sp"
                android:id="@+id/etNum1"
                android:layout_span="5"
                android:layout_weight="1"
                android:inputType="number"/>

        </TableRow>

        <TableRow>

            <EditText
                android:hint="숫자 2 입력"
                android:textSize="30sp"
                android:id="@+id/etNum2"
                android:layout_span="5"
                android:layout_weight="1"
                android:inputType="number"/>

        </TableRow>

        <TableRow>

            <Button
                android:text="0"
                android:textSize="20sp"
                android:id="@+id/btn0"
                android:layout_weight="1" />

            <Button
                android:text="1"
                android:textSize="20sp"
                android:id="@+id/btn1"
                android:layout_weight="1"/>

            <Button
                android:text="2"
                android:textSize="20sp"
                android:id="@+id/btn2"
                android:layout_weight="1"/>

            <Button
                android:text="3"
                android:textSize="20sp"
                android:id="@+id/btn3"
                android:layout_weight="1"/>

            <Button
                android:text="4"
                android:textSize="20sp"
                android:id="@+id/btn4"
                android:layout_weight="1"/>


        </TableRow>

        <TableRow>

            <Button
                android:text="5"
                android:textSize="20sp"
                android:id="@+id/btn5"
                android:layout_weight="1"/>

            <Button
                android:text="6"
                android:textSize="20sp"
                android:id="@+id/btn6"
                android:layout_weight="1"/>

            <Button
                android:text="7"
                android:textSize="20sp"
                android:id="@+id/btn7"
                android:layout_weight="1"/>

            <Button
                android:text="8"
                android:textSize="20sp"
                android:id="@+id/btn8"
                android:layout_weight="1"/>

            <Button
                android:text="9"
                android:textSize="20sp"
                android:id="@+id/btn9"
                android:layout_weight="1"/>


        </TableRow>

        <TableRow>

            <Button
                android:text="더하기"
                android:textSize="20dp"
                android:id="@+id/btnAdd"
                android:layout_span="5"
                android:layout_weight="1">
            </Button>

        </TableRow>

        <TableRow>

            <Button
                android:text="빼기"
                android:textSize="20dp"
                android:id="@+id/btnSub"
                android:layout_span="5"
                android:layout_weight="1">
            </Button>

        </TableRow>

        <TableRow>

            <Button
                android:text="곱하기"
                android:textSize="20dp"
                android:id="@+id/btnMul"
                android:layout_span="5"
                android:layout_weight="1">
            </Button>

        </TableRow>

        <TableRow>

            <Button
                android:text="나누기"
                android:textSize="20dp"
                android:id="@+id/btnDiv"
                android:layout_span="5"
                android:layout_weight="1">
            </Button>

        </TableRow>

        <TableRow>

            <EditText
                android:text="계산 결과 : "
                android:textSize="30sp"
                android:textColor="#FF0000"
                android:layout_span="5"
                android:layout_weight="1"
                android:id="@+id/tvResult"/>

        </TableRow>

    </TableLayout>

</LinearLayout>


```
```java
package com.example.and0524_table_layout;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    EditText etNum1, etNum2;
    Button btn0, btn1, btn2, btn3, btn4, btn5, btn6, btn7, btn8, btn9;
    Button btnAdd, btnSub, btnMul, btnDiv;
    TextView tvResult;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.calculator);

        etNum1 = findViewById(R.id.etNum1);
        etNum2 = findViewById(R.id.etNum2);
        btn0 = findViewById(R.id.btn0);
        btn1 = findViewById(R.id.btn1);
        btn2 = findViewById(R.id.btn2);
        btn3 = findViewById(R.id.btn3);
        btn4 = findViewById(R.id.btn4);
        btn5 = findViewById(R.id.btn5);
        btn6 = findViewById(R.id.btn6);
        btn7 = findViewById(R.id.btn7);
        btn8 = findViewById(R.id.btn8);
        btn9 = findViewById(R.id.btn9);
        btnAdd = findViewById(R.id.btnAdd);
        btnSub = findViewById(R.id.btnSub);
        btnMul = findViewById(R.id.btnMul);
        btnDiv = findViewById(R.id.btnDiv);
        tvResult = findViewById(R.id.tvResult);

        View.OnClickListener listener = new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                // 연산 버튼 클릭 시 입력받은 숫자 2개를 EditText로부터 가져오기
                // 단, 입력되지 않은 EditText 가 있을 경우 오류 메시지 출력 및 커서 요청
                if(etNum1.length() == 0) {
                    Toast.makeText(MainActivity.this, "첫 번째 숫자를 입력하세요!,", Toast.LENGTH_SHORT).show();
                    etNum1.requestFocus();
                    return;
                } else if(etNum2.length() == 0) {
                    Toast.makeText(MainActivity.this, "두 번째 숫자를 입력하세요!,", Toast.LENGTH_SHORT).show();
                    etNum2.requestFocus();
                    return;
                }

                // 모든 숫자가 정상적으로 입력되었을 경우(숫자 외의 문자 입력 판별 생략)
                // 입력받은 숫자 가져와서 변수에 저장

                int num1 = Integer.parseInt(etNum1.getText().toString());
                int num2 = Integer.parseInt(etNum2.getText().toString());

                switch (view.getId()) {
                    case R.id.btnAdd:
                        tvResult.setText("계산결과 : " + (num1 + num2));
                        break;
                    case R.id.btnSub:
                        tvResult.setText("계산결과 : " + (num1 - num2));
                        break;
                    case R.id.btnMul:
                        tvResult.setText("계산결과 : " + (num1 * num2));
                        break;
                    case R.id.btnDiv:

                        if(num2 == 0) { // 두번째 피연산자가0일 경우 오류 처리
                        Toast.makeText(MainActivity.this, "0으로 나눌 수 없음!", Toast.LENGTH_SHORT).show();
                        tvResult.setText("계산결과 : 오류 발생!");
                        return;
                    }
                        tvResult.setText("계산결과 : " + (num1 + num2));
                        break;
                }



            }
        };

        btnAdd.setOnClickListener(listener);
        btnSub.setOnClickListener(listener);
        btnMul.setOnClickListener(listener);
        btnDiv.setOnClickListener(listener);

        // -----------------------------------------------------------------
        // 0 ~ 9 까지의 숫자버튼에 대한 입력 처리
        View.OnClickListener btnNumberListener = new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                // 클릭된 위젯이 버튼 객체인 경우 Button 타입 변수에 저장
//                if(view instanceof Button) {
//                   Button btnNum = (Button)view;
//                }

                Button btnNum = (Button)view;

                // 현재 커서가 위치하고 있는 EditText 가져오기
                View v = getCurrentFocus();

                if(v == null) {
                    Toast.makeText(MainActivity.this, "입력할 위치를 선택하세요!", Toast.LENGTH_SHORT).show();
                    return;
                }

                EditText edit = (EditText) v;
                String strNum = edit.getText().toString();

                if(strNum.equals("0")) {
                    strNum = "";
                }

                strNum += btnNum.getText().toString();
                edit.setText(strNum);






//
//               // 현재 EditText에 커서 위치 여부 확인
//                if(etNum1.isFocused()) { // 숫자 1에 커서가 위치한 경우
//                    // 기존에 입력된 숫자 가져오기
//                    String strNum = etNum1.getText().toString();
//
//                    if(strNum.equals("0")) {
//                        strNum = "";
//                    }
//
//                    strNum += btnNum.getText().toString();
//                    etNum1.setText(strNum);
//                } else if(etNum2.isFocused()) {
//
//                    // 기존에 입력된 숫자 가져오기
//                    String strNum = etNum2.getText().toString();
//
//                    if(strNum.equals("0")) {
//                        strNum = "";
//                    }
//
//                    strNum += btnNum.getText().toString();
//                    etNum2.setText(strNum);
//
//                } else { // 숫자 입력란에 커서가 없는 경우
//                    Toast.makeText(MainActivity.this, "입력할 위치를 선택하세요!", Toast.LENGTH_SHORT).show();
//                    return;
//                }
            }
        };

        btn0.setOnClickListener(btnNumberListener);
        btn1.setOnClickListener(btnNumberListener);
        btn2.setOnClickListener(btnNumberListener);
        btn3.setOnClickListener(btnNumberListener);
        btn4.setOnClickListener(btnNumberListener);
        btn5.setOnClickListener(btnNumberListener);
        btn6.setOnClickListener(btnNumberListener);
        btn7.setOnClickListener(btnNumberListener);
        btn8.setOnClickListener(btnNumberListener);
        btn9.setOnClickListener(btnNumberListener);


    }
}
```

---

# [오후수업] JSP 71차

> - 저번 시간에 수업한 Test 프로젝트의 web.xml에서 다음 부븐을 복사 후 현재 진행중인 프로젝트의 web.xml에 붙여넣음.

```
<!-- POSt 방식 한글 처리를 위한 필터 설정 -->
	<filter>
		<filter-name>encodingFilter</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
		</init-param>
	</filter>
	<!-- POST 방식 한글 처리 필터와 서블릿 주소 패턴 매핑(모든 주소에 대해 UTF-8 필터링) -->
	<filter-mapping>
		<filter-name>encodingFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
```    

> - com.itwillbs.test.controller 패키지 생성 & BoardFrontController.java 생성

```java
-------------------------------BoardFrontController.java-------------------------------
package com.itwillbs.test.controller;

import java.util.ArrayList;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import com.itwillbs.test.vo.TestVO;

@Controller
public class BoardFrontController {

	@RequestMapping(value = "write.bo", method = RequestMethod.GET)
	public String write() {
		return "write_form";
	}

	// write_form.jsp 페이지에서 POST 방식 요청을 처리
	// => 위의 write() 메서드와 요청 주소는 동일하나 요청 방식(메서드)이 다르게 매핑
//	@RequestMapping(value = "write.bo", method = RequestMethod.POST)
//	public String writePost(String subject, String content) {
//		System.out.println(subject);
//		System.out.println(content);
//		// => 단, request 객체의 인코딩 타입이 UTF-8 타입이 아니므로 한글 등이 깨짐
//
//		return "";
//	}

	// write_form.jsp 페이지로부터 요청받은 폼 파라미터를 가져오기
	// => 별도의 파라미터 변수를 지정하는 대신 VO(DTO, Bean) 객체를 활용 가능
	// 메서드 정의 시 해당 파라미터와 일치하는 멤버변수를 갖는 VO 클래스를 지정하면 자동 주입됨
	// => 주의! 반드시 TestVO 클래스에 기본 생성자가 존재해야한다! 
	@RequestMapping(value = "write.bo", method = RequestMethod.POST)
	public String write(TestVO vo) { // subject, content 파라미터가 자동으로 객체에 저장됨
		System.out.println(vo.getSubject());
		System.out.println(vo.getContent());
		// => 단, request 객체의 인코딩 타입이 UTF-8 타입이 아니므로 한글 등이 깨짐

		// "list.bo" 서블릿 주소 요청
		return "redirect:/list.bo";
	}
	
	@RequestMapping(value = "list.bo", method = RequestMethod.GET)
	public String list(Model model) {
		// 데이터베이스 글목록 조회 작업은 생략 - 조회되었다고 가정
		// 1. 전체 글목록을 저장할 ArrayList 객체 생성
		//	  => 1개 게시물 정보를 저장하는 TestVO 타입을 제네릭타입으로 지정
		ArrayList<TestVO> testList = new ArrayList<TestVO>();
		
		// 2. 1개 게시물 정보를 저장하는 TestVO 객체 생성하여 데이터 저장
		// => TestVO 객체를 다시 ArrayList 객체에 추가
		testList.add(new TestVO("제목1", "내용1"));
		testList.add(new TestVO("제목2", "내용2"));

		// 3. 뷰페이지로 전달할 데이터를 Model 객체에 저장
		model.addAttribute("testList", testList);
		
		return "list";
	}
	
}


```

```jsp
-------------------------------write_form.jsp-------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<%@ include file="inc/top.jsp" %>
	<h1>write_form.jsp - 글쓰기폼</h1>
	<form action="write.bo" method="post">
		제목 <input type="text" name="subject"><br>
		내용 <input type="text" name="content"><br>
		<input type="submit" value="글쓰기">		
	</form>
	
	
</body>
</html>

-------------------------------list.jsp-------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>   
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>list.jsp - 글목록</h1>
	<%-- JSTL 과 EL 을 사용하여 ArrayList(testList) 객체에 저장된 TestVO 객체의 데이터 출력 --%>
	<%-- c:forEach items="전체 데이터 저장된 객체" var="1개 데이터를 저장할 객체 이름" --%>
	<c:forEach items="${testList }" var="test">
		<h3>제목 : ${test.subject } , 내용 : ${test.content }</h3>
	</c:forEach>
</body>
</html>

```
