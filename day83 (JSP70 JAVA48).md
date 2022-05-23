# [오전수업] JSP 70차
> 시작 전 console의 buffer size를 80000(디폴트값) 에서 1000000으로 수정
![캡처](https://user-images.githubusercontent.com/95197594/169722243-e79f8fe0-d4e5-4610-9dc7-7970d2b3776c.PNG)

> - Spring Legacy Project 생성
> - ./views/main.jsp 파일 생성 & ./views/inc 폴더 생성 후 그 외 파일들 생성
> - Server - Add and Remove에서 저번 시간에 수업한 Test 프로젝트를 제외.

![1](https://user-images.githubusercontent.com/95197594/169722805-378485fa-a084-47a0-a7c8-f43c9463ef42.png)
![2](https://user-images.githubusercontent.com/95197594/169722806-91a8c57e-ff61-4a78-bbd1-e4efa4fbbfb4.png)
![3](https://user-images.githubusercontent.com/95197594/169722966-45e80e7a-a245-4d0e-9545-8dd292a5644e.png)
![캡처](https://user-images.githubusercontent.com/95197594/169723198-aa585198-d288-4f93-828c-dd7f05cf64b7.PNG)


```java
-----------------------------------------------------TestController.java-----------------------------------------------------
package com.itwillbs.test.controller;

import java.net.URLEncoder;

import javax.servlet.http.HttpServletRequest;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

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
	public String rediect() {
		
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
		return "redirect:/dispatcher?name=" + name + "&age=" + age;
		
	}
	
	// 리다이렉트 방식 요청에 대한 처리를 수행하기 위해
	// Dispatcher 방식을 사용하여 redirect.jsp 페이지로 포워딩
	@RequestMapping(value = "dispatcher", method = RequestMethod.GET)
	public String dispatcher(String name, int age) {
		// 리다이렉트 방식 요청에서 URL 파라미터로 전달되는 name, age 값 자동 저장
		System.out.println("이름 : " + name);
		System.out.println("나이 : " + age); 
		return "redirect"; // "/WEB-INF/views/redirect.jsp"
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
</body>
</html>
```

---

# [오후수업] JAVA 48차
