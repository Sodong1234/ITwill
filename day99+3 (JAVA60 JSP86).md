# [오전수업] JAVA 60차
## Menu_ContextMenu
> - res 폴더에 New -> New Resource Directory에서 Resource type를 menu로 선택 후 생성
> - 생성한 menu 폴더에 menu1.xml, menu2.xml 생성

```xml
---------------------------------------------------activity_menu.xml---------------------------------------------------
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

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="원하는 메뉴를 표시할 버튼을 롱클릭"
        android:textSize="20sp"
        android:gravity="center"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/btn1"
        android:text="배경색 변경"
        android:textSize="20sp"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/btn2"
        android:text="이미지 변경"
        android:textSize="20sp"/>

    <ImageView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/iv"
        android:layout_marginTop="100dp"
        android:src="@drawable/ic_launcher"/>


</LinearLayout>
---------------------------------------------------menu1.xml---------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">

    <item
        android:id="@+id/itemRed"
        android:title="배경색(빨강)"/>

    <item
        android:id="@+id/itemGreen"
        android:title="배경색(초록)"/>

    <item
        android:id="@+id/itemBlue"
        android:title="배경색(파랑)"/>
</menu>
---------------------------------------------------menu2.xml---------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">

    <item
        android:id="@+id/itemRotate"
        android:title="이미지 회전"/>

    <item
        android:id="@+id/itemExpand"
        android:title="이미지 확대"/>

</menu>
```
```java
package com.example.and0621_menu_contextmenu;

import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;

import android.graphics.Color;
import android.os.Bundle;
import android.view.ContextMenu;
import android.view.MenuInflater;
import android.view.MenuItem;
import android.view.View;
import android.widget.Button;
import android.widget.ImageView;
import android.widget.LinearLayout;

public class MainActivity extends AppCompatActivity {

    ImageView iv;
    LinearLayout baseLayout;
    Button btn1, btn2;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        iv = findViewById(R.id.iv);
        baseLayout = findViewById(R.id.baseLayout);
        btn1 = findViewById(R.id.btn1);
        btn2 = findViewById(R.id.btn2);

        // 컨텍스트 메뉴를 위젯에 등록하기 위해서는
        // registForContextMenu() 메서드를 호출하여 파라미터로 위젯 객체를 전달
        // => 아직 어떤 메뉴가 표시될지는 결정되지 않은 상태
        registerForContextMenu(btn1); // btn1 버튼위젯에 컨텍스트 메뉴 등록
        registerForContextMenu(btn2); // btn2 버튼위젯에 컨텍스트 메뉴 등록


        }

    @Override
        public void onCreateContextMenu(ContextMenu menu, View v, ContextMenu.ContextMenuInfo menuInfo) {
        super.onCreateContextMenu(menu, v, menuInfo);

        // 메뉴 등록을 위해서 MenuInflater 객체 필요
        MenuInflater menuInflater = getMenuInflater();

        // 2개 이상의 위젯에 각각 다른 메뉴를 등록해야할 경우
        // 파라미터로 전달된 View 객체 (v)를 각 위젯과 비교하여 롱클릭된 위젯 판별해야함
        if (v == btn1) { // v.getId() == R.id.btn1 비교도 결과 동일함
            // 배경색 변경 버튼에 menu1.xml 연결
            menuInflater.inflate(R.menu.menu1, menu);
            menu.setHeaderTitle("배경색 선택");

        } else if (v == btn2) { // v.getId() == R.id.btn2 비교도 결과 동일함

            menuInflater.inflate(R.menu.menu2, menu);
            menu.setHeaderTitle("이미지 작업 선택");
        }

    }
    // onContextItemSelected() 메서드를 오버라이딩 선택된 메뉴에 대한 작업 수행
    @Override
    public boolean onContextItemSelected(@NonNull MenuItem item) {
        super.onContextItemSelected(item);

        switch (item.getItemId()) {
            case R.id.itemRed:
                baseLayout.setBackgroundColor(Color.RED);
                break;
            case R.id.itemGreen:
                baseLayout.setBackgroundColor(Color.GREEN);
                break;
            case R.id.itemBlue:
                baseLayout.setBackgroundColor(Color.BLUE);
                break;
            case R.id.itemRotate:
                iv.setRotation(iv.getRotation() + 30);
                break;
            case R.id.itemExpand:
                iv.setScaleX(iv.getScaleX() + 2);
                iv.setScaleY(iv.getScaleY() + 2);
                break;


        }
        return true;


    }
}
```

## Activity_Basic
> - layout 폴더에서 New -> Layout Resource Directory 클릭 후 second_activity.xml 생성
> - java 폴더에서 New -> Java Class 클릭 후 SecondActivity 생성

```xml
---------------------------------------------------activity_main.xml---------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical">

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/btnNewActivity"
        android:text="새 액티비티(화면) 열기"
        android:textSize="20sp"
        android:onClick="showNewActivity"/>

</LinearLayout>
---------------------------------------------------second_activity.xml---------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#FFFF88">

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/btnReturn"
        android:text="돌아가기"
        android:textSize="20sp"/>


</androidx.constraintlayout.widget.ConstraintLayout>
---------------------------------------------------AndroidManifest.xml---------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.and0621_activity_basic">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.And0621_Activity_Basic">
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <!-- 새로운 액티비티를 사용하기 위해서는 activity 태그로 액티비티 클래스 등록 필수! -->
        <activity android:name=".SecondActivity" android:label="두번째 액티비티"/>
    </application>

</manifest>
```
```java
---------------------------------------------------MainActivity.java---------------------------------------------------
package com.example.and0621_activity_basic;

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

    public void showNewActivity(View v) {
        // 현재 액티비티에서 새로운 액티비티를 실행(전환)하기 위해서는
        // Intent 객체를 생성하여 전환활 액티비티 클래스를 지정하고
        // startActivity() 메서드를 호출하여 생성된 Intent 객체를 전달
        // => 주의! 전환할 액티비티 클래스 지정 시 "클래스명.class" 형태로 지정!
        Intent intent = new Intent(MainActivity.this, SecondActivity.class);
        // => 현재 객체 : MainActivity, 전환할 액티비티 : SecondActivity 클래스
        // => 주의! 새로운 액티비티로 전환하기 위해서는 해당 액티비티 클래스를
        //    반드시 AndroidManifests.xml 파일 내에 등록해야한다!
        //    ex) <activity android:name=".SecondActivity" android:label="두번째 액티비티"/>
        startActivity(intent);

    }

}
---------------------------------------------------second_activity.java---------------------------------------------------
package com.example.and0621_activity_basic;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;

import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;

// 액티비티 클래스 정의 시 반드시 Activity 클래스 상속 필수!
// => 최근 사용하는 클래스는 Activity 클래스의 서브클래스인
//    androidx.appcompat.app.AppCompatActivity
public class SecondActivity extends AppCompatActivity {
    // 액티비티 클래스 사용을 위해서는 해당 액티비티가 생성(메모리에 로딩) 될 때
    // 자동으로 호출되는 onCreate() 메서드 오버라이딩 필수!


    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        // 액티비티 클래스 하나당 화면을 구성하는 레이아웃(XML 파일) 하나를 연결
        // => 연결을 위해 setContentView() 메서드를 호출하여
        //    액티비티 실행 시 표시할 레이아웃(XML) 파일을 저장

        setContentView(R.layout.second_activity);

        Button btnReturn = findViewById(R.id.btnReturn);

        btnReturn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                // 현재 액티비티를 종료하고 이전 액티비티로 돌아가기 위해서는
                // finish() 메서드 호출
                finish();
            }
        });
    }
}

```

## Activity_Basic2
> Activity_Basic과 같은 방식으로 

```xml
---------------------------------------------------activity_main.xml---------------------------------------------------
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
        android:text="새 액티비티(화면) 열기"
        android:textSize="20sp"/>

</LinearLayout>
---------------------------------------------------second_activity.xml---------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#FFFF88"
    android:orientation="vertical">

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/btnReturn"
        android:text="돌아가기"
        android:textSize="20sp"/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/btnThirdActivity"
        android:text="세번째 액티비티 열기"
       android:textSize="20sp"/>

</LinearLayout>
---------------------------------------------------third_activity.xml---------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#FF88FF">

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/btnReturn"
        android:text="돌아가기"
        android:textSize="20sp"/>



</LinearLayout>
---------------------------------------------------AndroidManifest.xml---------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.and0621_activity_basic2">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.And0621_Activity_Basic2">
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity android:name=".SecondActivity" android:label="두번째 액티비티"/>
        <activity android:name=".ThirdActivity" android:label="세번째 액티비티"/>
    </application>

</manifest>
```
```java
---------------------------------------------------MainActivity.java---------------------------------------------------
package com.example.and0621_activity_basic2;

import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button btnNewActivity = findViewById(R.id.btnNewActivity);
        btnNewActivity.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent intent =new Intent(MainActivity.this, SecondActivity.class);
                startActivity(intent);
            }
        });
    }
}
---------------------------------------------------SecondActivity.java---------------------------------------------------
package com.example.and0621_activity_basic2;

import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;

import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;

public class SecondActivity extends AppCompatActivity {
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.second_activity);

        Button btnReturn = findViewById(R.id.btnReturn);
        Button btnThirdActivity = findViewById(R.id.btnThirdActivity);

        btnReturn.setOnClickListener(view -> finish());

        btnThirdActivity.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent intent = new Intent(SecondActivity.this, ThirdActivity.class);
                startActivity(intent);
            }
        });
    }
}

---------------------------------------------------ThirdActivity.java---------------------------------------------------
package com.example.and0621_activity_basic2;

import android.os.Bundle;
import android.widget.Button;

import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;

public class ThirdActivity extends AppCompatActivity {
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.third_activity);

        Button btnReturn = findViewById(R.id.btnReturn);
        btnReturn.setOnClickListener(view -> finish());
    }
}

```

---

# [오후수업] JSP 86차
## 소켓 프로그래밍(Socket Programming)
### 소켓(Socket)
- 프로그램 상에서 통신을 수행하는 단말 장치의 논리적 끝단
- 소켓이 서로 연결되어 있는 단말 장치끼리 통신 수행 가능
- 소켓으로 통신을 하기 위해서는 네트워크 포트 하나를 사용해야함
  - (컴퓨터 상의 포트번호 : 0 ~ 65535번까지)
- 자바에서 소켓 프로그래밍을 위한 클래스 및 인터페이스를 제공
  - java.net 패키지 활용
  - 주로, java.io 패키지의 클래스들과 함께 사용하는 경우가 많음

### URL 클래스
- URL(주소) 정보를 관리하는 클래스
- URL(Uniform Resource Locator) 이란?
  - 웹(WWW) 상의 자원들의 위치를 가리키는 주소(= 포인터)
- URL 형식
  - ex) http://www.itwillbs.co.kr:80/index.jsp
    - http - 프로토콜(Protocol)
    - www.itwillbs.co.kr - 호스트명(주소)
    - 80 - 서비스 포트 번호
    - index.jsp - 자원 경로(Path) 및 파일명(File)

```java
package socket_programming;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.MalformedURLException;
import java.net.URL;
import java.net.URLConnection;
import java.util.Date;

public class Ex {

	public static void main(String[] args) {
		
		URL url = null;
		
		try {
			// URL 객체의 openConnection() 메서드를 활용하여 URLConnection 객체 얻을 수 있으며
			// URLConnection 객체의 connect() 메서드를 활용하여 해당 URL에 접근 가능
			
			url = new URL("https://itwillbs.co.kr/css/wvtex/img/wvUser/busan/logo.png");
//			url = new URL("https://itwillbs.co.kr/index.jsp");
//			url = new URL("http://192.168.3.200:8080/Test1/loginForm.jsp");
			// 주의! 이 URL 이 실제로 존재하는지 여부는 상관없다!
			
			// URL 객체로부터 메타데이터(= 부가적인 정보) 얻어오기
			System.out.println("프로토콜 : " + url.getProtocol());
			System.out.println("호스트명 : " + url.getHost());
			System.out.println("포트번호 : " + url.getDefaultPort());
			System.out.println("파일명 : " + url.getFile());

		} catch (MalformedURLException e) {
			e.printStackTrace();
		}
		
		try {
//			url = new URL("http://itwillb.co.k"); 
			// => 만약, URL 정보(자원 경로 제외)가 틀릴 경우 예외 발생할 수 있음
			// URL 객체에 지정된 URL 정보를 사용하여 해당 URL 에 접속(연결) 수행
			// => 접속 성공 시 URLConnection 객체 리턴됨
			URLConnection con = url.openConnection();
			con.connect(); // 자원의 경로를 제외한 주소부분이 틀릴 경우 예외 발생
			
			// 실제 해당 자원에 접근했을 경우 정보 가져오기
			System.out.println("문서타입 : " + con.getContentType());
			System.out.println("문서크기 : " + con.getContentLength() + " Byte");
			// => 만약, 해당 URL 에 자원이 존재하지 않을 경우 크기는 -1 리턴됨
			System.out.println("문서 최종 수정일 : " + new Date(con.getLastModified()));
			
		} catch (IOException e) {
			e.printStackTrace();
		}
		
		System.out.println("==========================================");
		// 아이티윌 부산교육센터 인덱스 페이지의 소스코드 읽어오기
		try {
			url = new URL("https://itwillbs.co.kr");
			
			// URL 객체로부터 InputStream 객체를 얻어와서
			// Byte -> Char -> String 형태로 최종 변환한 스트림을 통해 소스코드 한 줄씩 출력
			// 1. URL 객체의 openStream() 메서드를 호출하여 InputStream 객체 연결(Byte 단위 처리)
//			InputStream is = url.openStream();
			// 2. InputStream 객체를 char 단위로 처리하기 위해 InputStreamReader 객체 생성
//			InputStreamReader reader = new InputStreamReader(is);
			// 3. InputStreamReader 객체를 String 단위로 처리하기 위해 BufferedReader 객체 생성
//			BufferedReader buffer = new BufferedReader(reader);
			
			// 위의 세 문장을 한 문장으로 결합
			BufferedReader buffer = new BufferedReader(new InputStreamReader(url.openStream()));
			
			// 버퍼(BufferedReader)로부터 입력되는 입력스트림을 한 문장(라인) 읽어서 변수에 저장
			String str = buffer.readLine();
			
			// 읽어온 한 문장이 null 이 아닐 동안 반복해서 읽어오기
			while(str != null) {
				// 읽어온 문장 출력 후 다음 문장 읽어오기
				System.out.println(str);
				str = buffer.readLine();
			}
		} catch (MalformedURLException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}
		
		

	}

}
```

## URLEncoder, URLDecoder 클래스
- URL 에서 사용되는 문자열을 MIME 형식으로 변환하기 위한 클래스
  - (MIME : Multipurpose Internet Mail Extensions)
- 주로, 한글로 표시되는 쿼리 문자열 등이 웹 서버에 전송될 때의 형태로 특정 규칙에 따라 변환되어짐
  - 1) 아스키코드(영문 대문자, 소문자, 숫자, 일부 특수문자)는 그대로 사용
  - 2) 공백은 + 기호로 변환 
  - 3) 기타 문자(한글, 한자 등)는 %XX 형태의 16진수 2자리 문자로 변환됨

  - ex) 네이버 검색창에 "ITWILL 부산교육센터"를 검색했을 때 다음과 같은 URL 요청 발생
  - https://search.naver.com/search.naver?where=nexearch&sm=top_hty&fbm=0&ie=utf8&query=ITWILL+%EB%B6%80%EC%82%B0%EA%B5%90%EC%9C%A1%EC%84%BC%ED%84%B0
 	- 이 때, 주소창에 보이는 한글, 한자 등이 실제 주소 구성과 다르게 보일 수 있음

```java
package socket_programming;

import java.io.UnsupportedEncodingException;
import java.net.URLDecoder;
import java.net.URLEncoder;

public class Ex2 {

	public static void main(String[] args) {
	try {
			String originalStr = "[Java Programming : 양윤석]";
			System.out.println("원본 문자열 : " + originalStr);
			
			// URLEncoder 클래스의 encode() 메서드를 사용하여 원본 문자열을 MIME 형식으로 변환(= 인코딩)
			String encodeStr = URLEncoder.encode(originalStr, "UTF-8");
			System.out.println("MIME 형식 변환 후 : " + encodeStr);
			// MIME 형식 변환 후 : %5BJava+Programming+%3A+%EC%96%91%EC%9C%A4%EC%84%9D%5D
			
			// URLDecoder 클래스의 decode() 메서드를 사용하여 MIME 형식 문자열을 원래 문자열로 변환(= 디코딩)
			String decodeStr;
			decodeStr = URLDecoder.decode(encodeStr, "UTF-8");
			System.out.println("원본으로 복원 후 : " + decodeStr);
			// 원본으로 복원 후 : [Java Programming : 양윤석]
		} catch (UnsupportedEncodingException e) {
			// 지정된 인코딩(디코딩) 타입(UTF-8 등)이 지원되지 않을 경우 발생하는 예외
			e.printStackTrace();
		}
	}

}

```
