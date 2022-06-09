# [오전수업] JSP 80차
> - view 폴더에 token.jsp 생성 (후에 bank_main.jsp로 이름 변경)
> - com.itwillbs.fintech.vo / com.itwillbs.fintech.service 패키지 생성
>   - com.itwillbs.fintech.vo 패키지에 RequestTokenVO / ResponseTokenVO 클래스 생성
>   - com.itwillbs.fintech.service 패키지에 OpenBankingService / OpenBankingApiClient 클래스 생성



- controller
```java
--------------------------------------OpenBankingController.java--------------------------------------
package com.itwillbs.fintech.controller;

import javax.servlet.http.HttpServletRequest;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import com.itwillbs.fintech.service.OpenBankingService;
import com.itwillbs.fintech.vo.RequestTokenVO;
import com.itwillbs.fintech.vo.ResponseTokenVO;

@Controller
public class OpenBankingController {
	private String clientId = "0ded34b8-72a5-42d0-bb58-7af4dd683f41";
	private String clientSecret = "483c45ab-ea4f-4a3d-9902-80c90b522c47";

	// OpenBankingService 객체 자동 주입
	@Autowired 
	private OpenBankingService openBankingService;
	
	// @RequestMapping(value = "/callback", method = RequestMethod.GET) 대신
	// @GetMapping(value = "/callback") 사용 가능

	@RequestMapping(value = "/callback", method = RequestMethod.GET)
	public String getToken(@ModelAttribute RequestTokenVO requestToken, Model model) {
		// OAuth 인증 완료 후 전송되는 인증코드(code)를 자동으로 RequestTokenVO 객체에 저장
		System.out.println("인증코드 : " + requestToken.getCode());
		
		// 응답데이터로 전달받은 인증코드를 사용하여 엑세스토큰 발급 받기
		// OpenBankingService 객체의 requestToken() 메서드를 호출하여 엑세스토큰 발급 요청
		// => 파라미터 : RequestTokenVO 객체, 리턴타입 : ResponseTokenVO 객체(responseToken)
		
		ResponseTokenVO responseToken = openBankingService.requestToken(requestToken);
		
		// bank_main.jsp 페이지로 포워딩
		// => 이 때, 전달받은 토큰 정보(ResponseTokenVO 객체)를 함께 전달
		model.addAttribute("responseToken", responseToken);

		return "bank_main";
	}
	

}

```

- vo
```java

--------------------------------------RequestTokenVO.java--------------------------------------
package com.itwillbs.fintech.vo;

// 사용자 토큰발급 API(금융결제원 API 사이트 - 2.1.2. 토큰발급 API) 요청 데이터
public class RequestTokenVO {
	private String code;
	private String client_id;
	private String client_secret;
	private String redirect_uri;
	private String grant_type;
	
	public void setRequestToken(String client_id, String client_secret, String redirect_uri, String grant_type) {
		this.client_id = client_id;
		this.client_secret = client_secret;
		this.redirect_uri = redirect_uri;
		this.grant_type = grant_type;
	}
	
	public String getCode() {
		return code;
	}
	public void setCode(String code) {
		this.code = code;
	}
	public String getClient_id() {
		return client_id;
	}
	public void setClient_id(String client_id) {
		this.client_id = client_id;
	}
	public String getClient_secret() {
		return client_secret;
	}
	public void setClient_secret(String client_secret) {
		this.client_secret = client_secret;
	}
	public String getRedirect_uri() {
		return redirect_uri;
	}
	public void setRedirect_uri(String redirect_uri) {
		this.redirect_uri = redirect_uri;
	}
	public String getGrant_type() {
		return grant_type;
	}
	public void setGrant_type(String grant_type) {
		this.grant_type = grant_type;
	}
	
}

--------------------------------------ResponseTokenVO.java--------------------------------------

package com.itwillbs.fintech.vo;

// 사용자 토큰발급 API(금융결제원 API 사이트 - 2.1.2. 토큰발급 API) 응답 데이터
public class ResponseTokenVO {
	private String access_token;
	private String token_type;
	private int expires_in;
	private String refresh_token;
	private String scope;
	private String user_seq_no;
	
	
	public String getAccess_token() {
		return access_token;
	}
	public void setAccess_token(String access_token) {
		this.access_token = access_token;
	}
	public String getToken_type() {
		return token_type;
	}
	public void setToken_type(String token_type) {
		this.token_type = token_type;
	}
	public int getExpires_in() {
		return expires_in;
	}
	public void setExpires_in(int expires_in) {
		this.expires_in = expires_in;
	}
	public String getRefresh_token() {
		return refresh_token;
	}
	public void setRefresh_token(String refresh_token) {
		this.refresh_token = refresh_token;
	}
	public String getScope() {
		return scope;
	}
	public void setScope(String scope) {
		this.scope = scope;
	}
	public String getUser_seq_no() {
		return user_seq_no;
	}
	public void setUser_seq_no(String user_seq_no) {
		this.user_seq_no = user_seq_no;
	}

}
```

- service
```java
--------------------------------------OpenBankingService.java--------------------------------------
package com.itwillbs.fintech.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.itwillbs.fintech.vo.RequestTokenVO;
import com.itwillbs.fintech.vo.ResponseTokenVO;

@Service
public class OpenBankingService { // OpenBankingController - OpenBankingApiClient 객체 중간자 역할
	// OpenBankingApiClient 객체 자동 주입
	@Autowired 
	private OpenBankingApiClient openBankingApiClient;

	// 엑세스토큰 발급 요청을 위한 requestToken() 메서드 호출
	public ResponseTokenVO requestToken(RequestTokenVO requestToken) {
		return openBankingApiClient.requestToken(requestToken);
	}
}


--------------------------------------OpenBankingApiClient.java--------------------------------------
package com.itwillbs.fintech.service;

import org.springframework.http.HttpEntity;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpMethod;
import org.springframework.stereotype.Service;
import org.springframework.util.LinkedMultiValueMap;
import org.springframework.util.MultiValueMap;
import org.springframework.web.client.RestTemplate;

import com.itwillbs.fintech.vo.RequestTokenVO;
import com.itwillbs.fintech.vo.ResponseTokenVO;

@Service
public class OpenBankingApiClient {
	private String clientId = "0ded34b8-72a5-42d0-bb58-7af4dd683f41";
	private String clientSecret = "483c45ab-ea4f-4a3d-9902-80c90b522c47";
	private String redirectUri = "http://localhost:8080/fintech/callback_token";
	private String baseUrl = "https://testapi.openbanking.or.kr/v2.0/";
	
	// REST 방식의 API 요청에 사용할 클래스 타입 변수 선언
	private RestTemplate restTemplate; // REST 방식 요청 및 응답에 사용되는 클래스
	private HttpHeaders httpHeaders; // 헤더 정보를 관리할 클래스
	
	// 헤더에 엑세스 토큰을 추가하는 setHeaderAccessToken() 메서드 정의
	// => 파라미터 : 엑세스토큰, 리턴타입 : HttpHeaders
	public HttpHeaders setHeaderAccessToken(String access_token) {
		// HttpHeaders 객체의 add() 메서드를 호출하여 "항목", "값" 형태로 파라미터 전달
		httpHeaders.add("Authorization", "Bearer " + access_token);
		return httpHeaders;
	}
	
	// 2.1.2. 토큰 발급 API 요청 - Access Token 가져오기
	// 엑세스토큰 발급 요청 작업을 수행할 requestToken() 메서드 정의
	public ResponseTokenVO requestToken(RequestTokenVO requestToken) {
		// REST 방식 요청에 필요한 객체 생성
		restTemplate = new RestTemplate();
		httpHeaders = new HttpHeaders();
		
		/* 요청 메세지 URL
		 * HTTP URL : https://openapi.openbanking.or.kr/oauth/2.0/token
		 * HTTP Method : POST
		 * Content-Type : application/x-www-form-urlencoded; charset=UTF-8
		 */
		// 1. HTTP Header 오브젝트(정보) 생성
		httpHeaders.add("Content-Type", "application/x-www-form-urlencoded; charset=UTF-8");
		
		// 2. HTTP Body 오브젝트(정보) 생성
		// 2-1. Body 에 추가할 데이터를 관리하는 RequestTokenVO 객체에 필요한 데이터 저장
		// => grant_type 값은 "authorization_code" 값 고정
		requestToken.setRequestToken(clientId, clientSecret, redirectUri, "authorization_code");
		
		// 헤더의 content-type 이 application/x-www-form-urlencoded 이므로 객체 저장이 불가능하며
		// 대신 객체 형태를 파라미터 형태로 변환하여 관리할 MultiValueMap 객체 사용
		MultiValueMap<String, String> parameters = new LinkedMultiValueMap<String, String>();
		// MultiValueMap 객체의 add() 메서드를 호출하여 키, 값 형태로 파라미터 데이터 저장
		parameters.add("code", requestToken.getCode());
		parameters.add("client_id", requestToken.getClient_id());
		parameters.add("client_secret", requestToken.getClient_secret());
		parameters.add("redirect_uri", requestToken.getRedirect_uri());
		parameters.add("grant_type", requestToken.getGrant_type());

		// HttpHeader 와 HttpBody 오브젝트를 하나의 객체로 관리하기 위한 HttpEntity 객체 생성
		// => 제네릭타입은 파라미터 데이터를 관맇나ㅡㄴ MultiValueMap<String, String> 타입 지정
		HttpEntity<MultiValueMap<String, String>> param = new HttpEntity<MultiValueMap<String,String>>(parameters, httpHeaders);
		
		// 엑세스토큰 요청에 사용될 요청 URL 저장
		String requestUrl = "https://testapi.openbanking.or.kr/oauth/2.0/token";
		
		// RestTemplate 객체의 exchange() 메서드를 호출하여 REST 방식 요청 작업 수행하고
		// 리턴되는 결과값(응답데이터)의 바디 영역 데이터를 리턴 
		// => exchange(요청 URL, HttpMethod.요청메서드, HttpEntity 객체, 응답받을 객체의 클래스타입).getBody();
		// => 이 때, 리턴되는 데이터 타입은 응답받을 객체의 클래스타입으로 
		// 	  Body 데이터가 자동으로 저장되어 리턴됨 
		return restTemplate.exchange(requestUrl, HttpMethod.POST, param, ResponseTokenVO.class).getBody();
	}
	
}

```
```jsp
--------------------------------------bank_main.jsp--------------------------------------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h3>인증완료</h3>
	<h3>엑세스 토큰 : ${responseToken.access_token }</h3>
	<h3>사용자 번호 : ${responseToken.user_seq_no }</h3>
	
	
	<form method="post" action="https://testapi.openbanking.or.kr/oauth/2.0/token" enctype="multipart/form-data">
			<%-- 필요 파라미터는 입력데이터 없이 hidden 속성으로 --%>
			<input type="hidden" name="code" value="${param.code }">
			<input type="hidden" name="client_id" value="${clientId }">
			<input type="hidden" name="client_secret" value="${clientSecret }">
			<input type="hidden" name="redirect_uri" value="http://localhost:8080/fintech/callback_token">
			<input type="hidden" name="grant_type" value="authorization_code">
			<input type="submit" value="토큰발급">
		</form>
</body>
</html>
```

---

# [오후수업] JAVA 55차
