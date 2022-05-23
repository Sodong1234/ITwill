# [오전수업] JSP 70차
> 시작 전 console의 buffer size를 80000(디폴트값) 에서 1000000으로 수정
![캡처](https://user-images.githubusercontent.com/95197594/169722243-e79f8fe0-d4e5-4610-9dc7-7970d2b3776c.PNG)

> - Spring Legacy Project 생성
> - ./views/main.jsp 파일 생성 & ./views/inc 폴더 생성 후 그 외 파일들 생성
> - com.itwillbs.test.vo 패키지에 TestVo.java 생성
> - Server - Add and Remove에서 저번 시간에 수업한 Test 프로젝트를 제외.

![1](https://user-images.githubusercontent.com/95197594/169722805-378485fa-a084-47a0-a7c8-f43c9463ef42.png)
![2](https://user-images.githubusercontent.com/95197594/169722806-91a8c57e-ff61-4a78-bbd1-e4efa4fbbfb4.png)
![3](https://user-images.githubusercontent.com/95197594/169722966-45e80e7a-a245-4d0e-9545-8dd292a5644e.png)
![캡처](https://user-images.githubusercontent.com/95197594/169723198-aa585198-d288-4f93-828c-dd7f05cf64b7.PNG)


```java
-----------------------------------------------------TestController.java-----------------------------------------------------
package com.itwillbs.test.controller;

import java.io.UnsupportedEncodingException;
import java.net.URLDecoder;
import java.net.URLEncoder;
import java.util.HashMap;
import java.util.Map;

import javax.servlet.http.HttpServletRequest;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.servlet.ModelAndView;

import com.itwillbs.test.vo.TestVO;

@Controller
public class TestController {
	
	@RequestMapping(value = "main", method = RequestMethod.GET)
	public String main() {
		return "main"; // "/WEB-INF/views/main.jsp"
	}
	
	// "push" 서블릿 주소 요청 시 HttpServletRequest 객체를 자동으로 전달받기(= 자동 주입)
	// => 메서드 정의 시 메서드 파라미터로 HttpServletRequest 타입 변수 선언
//	@RequestMapping(value = "push", method = RequestMethod.GET)
//	public String push(HttpServletRequest request) {
//		// HttpServletRequest 객체의 setAttribute() 메서드를 호출하여 객체(데이터) 저장 가능
//		request.setAttribute("msg", "Hello, World!");
//		// Dispatcher 방식 포워딩 수행 시 request 객체가 그대로 유지됨(저장 데이터도 유지)
//		return "push_data"; // "/WEB-INF/views/push_data.jsp"
//	}
	
	@RequestMapping(value = "push", method = RequestMethod.GET)
	public String push(Model model) {
		// org.springframework.ui.Model 타입을 파라미터로 지정 시 
		// 데이터 저장이 가능한 객체를 자동으로 전달받을 수 있음
		// => request 객체와 성격이 유사하며, java.util.Map 객체를 기반으로 만든
		//	  스프링에서 제공하는 데이터 공유 객체
		// => request.settAttribte() 메서드와 마찬가지로 Model 객체의 addAttribute() 로 데이터 저장
		// => request 객체와 범위(Scope) 동일
		// => request 객체와 동시 사용 불가(일반적인 데이터 저장 시 request 객체보다 더 많이 사용)
		model.addAttribute("msg", "Hello, world! - Model 객체");
		
		// Dispatcher 방식 포워딩 수행 시 request 객체가 그대로 유지됨(저장 데이터도 유지)
		return "push_data"; // "/WEB-INF/views/push_data.jsp"
	}
	
	
	@RequestMapping(value = "redirect", method = RequestMethod.GET) 
	public String rediect() throws UnsupportedEncodingException {
		
//		return "redirect"; // "/WEB-INF/views/redirect.jsp" => Dispatcher 방식 포워딩
		
		// 리다이렉트 방식 포워딩 시 "redirect:포워딩 할 주소" 형식으로 지정
//		return "redirect:/dispatcher";
		// => "dispatcher" 서블릿 요청에 대한 Dispatcher 방식 포워딩을 추가로 설정해야함
		
		// 테스트를 위해 URL 을 통해 파라미터 전달
//		String name = "hong";
//		int age = 20;
//		return "redirect:/dispatcher?name=" + name + "&age=" + age;
		
//		String name = "홍길동";
		int age = 20;
		// 주의! URL 을 통해 파라미터 직접 전달 시 한글, 한자 등 포함될 경우 깨짐
		// => 따라서, java.net.URLEncoder 클래스의 encode() 메서드를 호출하여 데이터 인코딩 필요
		String name = URLEncoder.encode("홍길동", "UTF-8"); // UTF-8 형식으로 데이터 인코딩 
//		return "redirect:/dispatcher?name=" + name + "&age=" + age;
		// http://localhost:8080/test/dispatcher?name=%ED%99%8D%EA%B8%B8%EB%8F%99&age=20	
		
		// @RequestParam 어노테이션 테스트용(age 파라미터 삭제)
		return "redirect:/dispatcher?name=" + name;
	}
	
	// 리다이렉트 방식 요청에 대한 처리를 수행하기 위해
	// Dispatcher 방식을 사용하여 redirect.jsp 페이지로 포워딩
//	@RequestMapping(value = "dispatcher", method = RequestMethod.GET)
//	public String dispatcher(Model model, String name, int age) throws UnsupportedEncodingException {
//		// 리다이렉트 방식 요청에서 URL 파라미터로 전달되는 name, age 값 자동 저장
//		// => 만약, 내부에서 Model 객체도 사용해야할 경우 순서 상관없이 Model 타입 변수 선언
//		System.out.println("이름 : " + name);
//		System.out.println("나이 : " + age); 
//		
//		// -------------------------------------------------------------------------------
//		// http://localhost:8080/test/dispatcher?name=%ED%99%8D%EA%B8%B8%EB%8F%99&age=20
//		// => 주의! 웹브라우저 URL 주소표시줄에는 파라미터가 한글로 표시되지만
//		//	  실제로는 인코딩 된 Unicode(3Byte) 로 되어 있음(주소표시줄 복사하면 위의 주소처럼 보임)
//		// => 만약, 인코딩 된 데이터를 다시 원래 형식으로 되돌려야할 경우 디코딩 작업 필요
//		//	  (단, 현재 저장된 파라미터를 메서드 내에서 사용하거나 Model 객체에 저장 시 디코딩 불필요)
//		// => URLDecoder 객체의 decode() 메서드를 호출하여 디코딩 수행
////		String decodedName = URLDecoder.decode(name, "UTF-8");
////		System.out.println(name + ", " + decodedName);
//		// -------------------------------------------------------------------------------
//
//		// Model 객체(데이터 공유 객체)에 이름, 나이를 저장
//		model.addAttribute("name2", name);
//		model.addAttribute("age2", age);
//		
//		
//		
//		return "redirect"; // "/WEB-INF/views/redirect.jsp"
//	}
	
	
	// 서블릿 요청 메서드 정의 시 @RequestParam 어노테이션을 사용하여 
	// 파라미터 데이터라는 표시를 명확하게 지정 가능
//	@RequestMapping(value = "dispatcher", method = RequestMethod.GET)
//	public String dispatcher(Model model, @RequestParam String name, @RequestParam int age) {
//		model.addAttribute("name2", name);
//		model.addAttribute("age2", age);
//		return "redirect"; // "/WEB-INF/views/redirect.jsp"
		
	// 메서드 정의 시 기술한 파라미터가 전달받은 파라미터에 존재하지 않을 경우를 대비하여
	// @RequestParam 어노테이션 뒤에 defaultValue 속성을 추가하여 기본값 설정 가능
	// ex) @RequestParam(defaultValue = "기본값") 변수 선언
	@RequestMapping(value = "dispatcher", method = RequestMethod.GET)
	public String dispatcher(Model model, @RequestParam String name, @RequestParam(defaultValue = "0") int age) {
		// => 만약, age 라는 파라미터가 존재하지 않으면 기본값 0 으로 설정
		model.addAttribute("name2", name);
		model.addAttribute("age2", age);
		
		
		return "redirect"; // "/WEB-INF/views/redirect.jsp"
		
	}
	
	// ModelANdView : 데이터를 저장하는 Model 객체 관리와 view 페이지 포워딩 처리를 함께 수행하는 객체
	@RequestMapping(value = "mav", method = RequestMethod.GET)
	public ModelAndView modelAndView() { // 메서드 리턴타입을 ModelAndView 타입으로 지정
		// 데이터를 저장하기 위해 HashMap 객체 활용
		Map<String, TestVO> map = new HashMap<String, TestVO>();
		map.put("key", new TestVO("제목", "내용"));
		
		// 만약, Model 객체가 아닌 다른 객체 자체를 뷰페이지로 전송하려면
		// ModelAndView 객체를 활용하여 전송할 객체를 지정할 수 있다!
		// => 기본 객체 생성 방법 : ModelAndView("이동할 페이지", "객체의 키", 객체값);
		// => 포워딩(이동) 페이지 지정 방법은 동일(Dispatcher)
		return new ModelAndView("model_and_view", "map", map);
	}
}


-----------------------------------------------------TestVO.java-----------------------------------------------------
package com.itwillbs.test.vo;

public class TestVO {
	private String subject;
	private String content;
	
	public TestVO() {}
	
	public TestVO(String subject, String content) {
		super();
		this.subject = subject;
		this.content = content;
	}

	public String getSubject() {
		return subject;
	}

	public void setSubject(String subject) {
		this.subject = subject;
	}

	public String getContent() {
		return content;
	}

	public void setContent(String content) {
		this.content = content;
	}
	
	
	
}

```
```jsp
-----------------------------------------------------main.jsp-----------------------------------------------------
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
	<h1>main.jsp</h1>
</body>
</html>
-----------------------------------------------------top.jsp-----------------------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<div>
	<a href="/test">홈페이지</a>
	<a href="/test/main">메인페이지</a>
	<a href="/test/push">데이터 전달</a>
	<a href="/test/redirect">리다이렉트</a>
	<a href="/test/mav">ModelAndView</a>
	<a href="/test/write.bo">글쓰기</a>	
	<a href="/test/list.bo">글목록</a>	
	<a href="/test/login.me">로그인</a>			
</div>
-----------------------------------------------------push_data.jsp-----------------------------------------------------
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
	<h1>push_data.jsp</h1>
	<%-- Dispatcher 방식 포워딩 시 함께 전달된 request 객체의 msg 속성값 가져오기 --%>
	<%-- Dispatcher 방식 포워딩 시 함께 전달된 Model 객체의 msg 속성값 가져오기(request 객체와 동일) --%>
	<h3>${msg }</h3>
</body>
</html>
-----------------------------------------------------redirect.jsp-----------------------------------------------------
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
	<h1>redirect.jsp</h1>
	<h3>이름 : ${name2 }</h3>
	<h3>나이 : ${age2 }</h3>
</body>
</html>
-----------------------------------------------------model_and_view.jsp-----------------------------------------------------
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
	<h1>model_and_view.jsp</h1>
	<%-- Map 객체("map") 내의 TestVO 객체("key") 내의 Getter 메서드를 통해 데이터 가져오기 --%>
	<h3>제목 : ${map.key.getSubject() }</h3>
	<h3>내용 : ${map.key.getContent() }</h3>
</body>
</html>


```

---

# [오후수업] JAVA 48차

## 중첩 리니어 레이아웃
- 위젯의 배치 방향을 서로 다르게 지정해야하는 경우 하나의 리니어레이아웃에서 배치가 불가능하며 각각의 리니어 레이아웃을 따로 생성하여 배치 방향을 다르게 해야함

```xml
-------------------------------------------------------activity_main.xml-------------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical">

    <!--
    중첩 리니어 레이아웃
    - 위젯의 배치 방향을 서로 다르게 지정해야하는 경우
    하나의 리니어레이아웃에서 배치가 불가능하며
    각각의 리니어 레이아웃을 따로 생성하여 배치 방향을 다르게 해야한다!
    -->
    <!--
    1. 레이아웃 내의 버튼을 수직 방향 배치, 중앙에 배치
    => 수직 배치 : orientation="vertical"
    => 중앙 배치 : gravity="center"
    -->

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        android:layout_weight="1"
        android:gravity="center"
        android:background="#FFFF88">

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="버튼1"
            android:textSize="30sp"/>

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="버튼2"
            android:textSize="30sp"/>


    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="horizontal"
        android:layout_weight="1"
        android:gravity="center"
        android:background="#FF88FF">

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="버튼3"
        android:textSize="30sp"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="버튼4"
        android:textSize="30sp"/>

    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_weight="1"
        android:gravity="center"
        android:background="#88FFFF"
        android:orientation="vertical">

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="버튼5"
            android:textSize="30sp"/>

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="버튼6"
            android:textSize="30sp"/>

    </LinearLayout>


</LinearLayout>
-------------------------------------------------------duplication_linearlayout_test.xml-------------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:orientation="horizontal">

        <LinearLayout
            android:layout_width="0dp"
            android:layout_height="match_parent"
            android:layout_weight="1"
            android:background="#FF0000">

        </LinearLayout>

        <LinearLayout
            android:layout_width="0dp"
            android:layout_height="match_parent"
            android:layout_weight="1"
            android:orientation="vertical">

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="0dp"
                android:layout_weight="1"
                android:background="#FFFF00">

            </LinearLayout>

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="0dp"
                android:layout_weight="1"
                android:background="#000000">

            </LinearLayout>

        </LinearLayout>

    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:background="#0000FF">

    </LinearLayout>
</LinearLayout>
```

