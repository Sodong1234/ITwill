# [오전수업] JAVA 57차
## Dialog

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical"
    android:gravity="center">

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/btnDialog"
        android:text="다이얼로그 표시"
        android:textSize="20sp"
        android:onClick="showDialog"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/btnDialog2"
        android:text="다이얼로그 표시2"
        android:textSize="20sp"
        android:onClick="showDialog2"/>


</LinearLayout>
```
```java
package com.example.and0614_dialog;

import androidx.appcompat.app.AppCompatActivity;

import android.app.AlertDialog;
import android.content.DialogInterface;
import android.os.Bundle;
import android.view.View;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        /*
         *  대화상자(Dialog)
         *  - 사용자에게 알림을 전달하거나, 사용자로부터 상호작용을 수행하는 별도의 창
         *  - AlertDialog.Builder 클래스 사용하여 다이얼로그 생성하고,
         *  setXXX() 메서드를 사용하여 다이얼로그 설정하고,
         *  show() 메서드를 사용하여 다이얼로그를 표시
         *  */
    }

    public void showDialog(View v) {
        // 버튼 클릭 시 다이얼로그 표시
        // 1. AlertDialog.Builder 클래스를 활용하여 다이얼로그 객체 생성
        // => 파라미터로 Context 객체 전달 (this 또는 MainActivity.this 또는 getAplicationContext())
        AlertDialog.Builder dialog = new AlertDialog.Builder(this);

        // 2. AlertDialog.Builder 객체의 setXXX() 메서드를 호출하여 다이얼로그 설정 작업 수행
        // 2-1. 다이얼로그에 표시할 제목 설정
        dialog.setTitle("다이얼로그 제목");
        // 2-2. 다이얼로그에 표시할 내용 설정
        dialog.setMessage("다이얼로그 본문");
        // 2-3. 아이콘 설정
        dialog.setIcon(R.mipmap.ic_launcher_round);

        // 2-4. 다이얼로그에 버튼 부착 (기본 버튼 3개 제공됨)
        // => 메서드에 따른 버튼 역할의 차이는 없으며, 관례적인 의미에 맞게 사용
//        dialog.setPositiveButton("확인", null);   // 긍정의미
//        dialog.setNegativeButton("취소", null);   // 부정의미
//        dialog.setNeutralButton("중립버튼", null); // 중립

        // 다이럴로그 버튼에 이벤트 연결
        // => 일반 Button 객체의 OnClickListener 와 동일한 방법으로 이벤트 처리 가능
        //     단, DialogInterface.OnclickListener 사용하는 점이 다름
        dialog.setPositiveButton("확인", new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialogInterface, int i) {
                Toast.makeText(MainActivity.this, "확인 버튼 누름!", Toast.LENGTH_SHORT).show();
            }
        });

        // 3. AlertDialog.Builder 객체의 show() 메서드를 호출하여 다이얼로그 표시
        dialog.show();
    }

    // showDialog2() 메서드 정의
    // 제목 : 영화 정보
    // 내용 : 영화 줄거리...............
    // 아이콘 : 영화 포스터 이미지
    // 버튼 : 상세보기        (취소) 확인

    public void showDialog2(View v) {
        AlertDialog.Builder dialog = new AlertDialog.Builder(this);

        dialog.setTitle("마더");
        dialog.setMessage("줄거리 모름");
        dialog.setIcon(R.drawable.mov19);

        // 4단계로 구현된 리스너 연결 시
        dialog.setNeutralButton("상세보기", dialogBtnListener);
        dialog.setNegativeButton("취소", dialogBtnListener);
        dialog.setPositiveButton("확인", dialogBtnListener);

        dialog.show();
    }

    // 다이얼로그 내의 버튼에 리스너 연결 4단계 구현 시
    DialogInterface.OnClickListener dialogBtnListener = new DialogInterface.OnClickListener() {
        @Override
        public void onClick(DialogInterface dialogInterface, int i) {
            Toast.makeText(MainActivity.this, i + "", Toast.LENGTH_SHORT).show();
            // 파라미터로 전달받은 i 값을 사용하여 확인 버튼과 상세보기 버튼 구별
            // PositiveButton : -1
            // NegativeButton : -2
            // NeutralButton  : -3
            
            switch (1) {
                case -1:
                    Toast.makeText(MainActivity.this, "확인 클릭됨!", Toast.LENGTH_SHORT).show();
                    break;
                case -2:
                    Toast.makeText(MainActivity.this, "취소 클릭됨!", Toast.LENGTH_SHORT).show();
                    break;
                case -3:
                    Toast.makeText(MainActivity.this, "상세보기 클릭됨!", Toast.LENGTH_SHORT).show();
                    break;
            }
        }
    };

}
```

## Dialog 2

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical">

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/btnListDialog"
        android:text="목록 다이얼로그 표시"
        android:textSize="20sp"
        android:onClick="showListDialog"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/btnRadioDialog"
        android:text="라디오버튼 목록 다이얼로그 표시"
        android:textSize="20sp"
        android:onClick="showRadioListDialog"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/btnCheckListDialog"
        android:text="체크박스 목록 다이얼로그 표시"
        android:textSize="20sp"
        android:onClick="showCheckListDialog"/>

</LinearLayout>
```
```java
package com.example.and0614_dialog2;

import androidx.appcompat.app.AppCompatActivity;

import android.app.AlertDialog;
import android.content.DialogInterface;
import android.os.Bundle;
import android.view.View;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    int selectedItem = -1;  // 선택 항목에 대한 인덱스를 저장할 변수 선언

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
//  showListDialog
    public void showListDialog(View v) {
        AlertDialog.Builder dialog = new AlertDialog.Builder(this);
                
        dialog.setTitle("좋아하는 과목은?");
        dialog.setIcon(R.mipmap.ic_launcher_round);
     
        // 목록 대화상자 출력을 위해 목록으로 사용할 데이터를 배열로 생성
        String[] listItems = {"Java", "JSP", "Android", "SPRING"};

        // AlertDialog.Builder 객체의 setItems() 메서드를 호출하여 목록 생성
        // => 파라미터 : 목록으로 사용할 String 배열, 리스너 객체
        //    => setXXXButton의 리스너와 동일한 객체 구현하여 이벤트 처리
        dialog.setItems(listItems, new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialogInterface, int i) {
                // 파라미터로 전달되는 i 값을 사용하여 목록 중 선택된 항목을 판별
//                Toast.makeText(MainActivity.this, i + "", Toast.LENGTH_SHORT).show();
                // => i 값은 배열 인덱스와 동일한 순서로 전달되므로 배열 항목의 인덱스로 활용
                Toast.makeText(MainActivity.this, listItems[i], Toast.LENGTH_SHORT).show();
                selectedItem = i;
            }
        });
        
        dialog.setPositiveButton("확인", null);
        dialog.show();
    }
//  showRadioListDialog
    public void showRadioListDialog(View v) {
        AlertDialog.Builder dialog = new AlertDialog.Builder(this);

        dialog.setTitle("좋아하는 과목은?");
        dialog.setIcon(R.mipmap.ic_launcher_round);

        String[] listItems = {"Java", "JSP", "Android", "SPRING"};
        
        // AlertDialog.Builder 객체의 setSingleChoiceItems() 메서드를 호출하여
        // 라디오버튼 형태의 단일 선택 목록 생성
        // => 파라미터 : 목록으로 사용할 String 배열, 초기 선택 항목 인덱스, 리스너 객체
        //    => setXXXButton 의 리스너와 동일한 객체 구현하여 이벤트 처리
        dialog.setSingleChoiceItems(listItems, 0, new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialogInterface, int i) {
//                Toast.makeText(MainActivity.this, listItems[i], Toast.LENGTH_SHORT).show();
                selectedItem = i;
            }
        });

        // 확인 버튼 클릭 시 선택된 라디오버튼의 항목 출력
        // => 라디오버튼 선택 시 이벤트 처리에서 저장된 selectedItem 변수값 활용
        dialog.setPositiveButton("확인", (dialogInterface, i) -> {
            Toast.makeText(MainActivity.this, listItems[selectedItem] + " 선택됨", Toast.LENGTH_SHORT).show();
        });
        
        dialog.show();
    }
//  showCheckListDialog
    public void showCheckListDialog(View v) {
        AlertDialog.Builder dialog = new AlertDialog.Builder(this);

        dialog.setTitle("좋아하는 과목은?");
        dialog.setIcon(R.mipmap.ic_launcher_round);

        String[] listItems = {"Java", "JSP", "Android", "SPRING"};
        boolean[] checkItems = {true, true, false, true};

        // AlertDialog.Builder 객체의 setMultiChoiceItems() 메서드 호출하여
        // 체크박스 형태의 다중 선택 목록 생성
        // => 파라미터 : 목록으로 사용할 String 배열, 초기 선택 항목 boolean 배열, 리스너 객체
        //    => OnMultiChoiceClickListener 구현하여 이벤트 처리
        dialog.setMultiChoiceItems(listItems, checkItems, new DialogInterface.OnMultiChoiceClickListener() {
            @Override
            public void onClick(DialogInterface dialogInterface, int i, boolean isChecked) {
                // 특정 항목(i)의 체크 상태(isChecked)를 boolean 타입 배열에 저장
                checkItems[i] = isChecked;
            }
        });

        // 확인 버튼 클릭 시 선택된 라디오 버튼의 항목 출력
        dialog.setPositiveButton("확인", new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialogInterface, int index) {
                // checkItems 배열의 체크 여부를 확인하여 체크상태(true)인 항목의 내용(listItems)을
                // 문자열로 결합하여 출력
                String items = "";

                // 반복문을 사용하여 체크 상태 여부를 배열로부터 확인 후
                // 아이템 항목의 인덱스를 통해 문자열 결합
                for(int i = 0; i < checkItems.length; i++) {
                    if(checkItems[i]) {
                        items += listItems[i] + " ";
                    }
                }

                Toast.makeText(MainActivity.this, items + " 체크됨", Toast.LENGTH_SHORT).show();
            }
        });
        dialog.show();
    }


}

```
---

# [오후수업] JSP 82차

> - Maven Repository에서 JSON.simply 1.1.1 / Gson 2.9.0 버전 라이브러리를 pom.xml에 추가
> - pom.xml springframework-version을 3.1.1.RELEASE에서 5.3.20으로 변경
> - vo 폴더에 UserInfoRequestVO.java & UserInfoResponseVO.java & AccountVO.java 파일 생성
> - views 폴더에 account 폴더 생성, 그 안에 user_info.jsp 파일 생성

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.itwillbs</groupId>
	<artifactId>fintech</artifactId>
	<name>FinTech</name>
	<packaging>war</packaging>
	<version>1.0.0-BUILD-SNAPSHOT</version>
	<properties>
		<java-version>1.8</java-version>
		<org.springframework-version>5.3.20</org.springframework-version>
		<org.aspectj-version>1.6.10</org.aspectj-version>
		<org.slf4j-version>1.6.6</org.slf4j-version>
	</properties>
	<dependencies>
		<!-- Spring -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>${org.springframework-version}</version>
			<exclusions>
				<!-- Exclude Commons Logging in favor of SLF4j -->
				<exclusion>
					<groupId>commons-logging</groupId>
					<artifactId>commons-logging</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>

		<!-- AspectJ -->
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjrt</artifactId>
			<version>${org.aspectj-version}</version>
		</dependency>

		<!-- Logging -->
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-api</artifactId>
			<version>${org.slf4j-version}</version>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>jcl-over-slf4j</artifactId>
			<version>${org.slf4j-version}</version>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>${org.slf4j-version}</version>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>log4j</groupId>
			<artifactId>log4j</artifactId>
			<version>1.2.15</version>
			<exclusions>
				<exclusion>
					<groupId>javax.mail</groupId>
					<artifactId>mail</artifactId>
				</exclusion>
				<exclusion>
					<groupId>javax.jms</groupId>
					<artifactId>jms</artifactId>
				</exclusion>
				<exclusion>
					<groupId>com.sun.jdmk</groupId>
					<artifactId>jmxtools</artifactId>
				</exclusion>
				<exclusion>
					<groupId>com.sun.jmx</groupId>
					<artifactId>jmxri</artifactId>
				</exclusion>
			</exclusions>
			<scope>runtime</scope>
		</dependency>

		<!-- @Inject -->
		<dependency>
			<groupId>javax.inject</groupId>
			<artifactId>javax.inject</artifactId>
			<version>1</version>
		</dependency>

		<!-- Servlet -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>servlet-api</artifactId>
			<version>2.5</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>javax.servlet.jsp</groupId>
			<artifactId>jsp-api</artifactId>
			<version>2.1</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
			<version>1.2</version>
		</dependency>



		<!-- Test -->
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.7</version>
			<scope>test</scope>
		</dependency>

		<!-- 추가 라이브러리 등록 -->
		<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>8.0.29</version>
		</dependency>

		<!-- RestTemplate 객체 사용하여 JSON 응답 처리할 때 필요한 라이브러리(jsoin-simple, gson) -->
		<!-- https://mvnrepository.com/artifact/com.googlecode.json-simple/json-simple -->
		<dependency>
			<groupId>com.googlecode.json-simple</groupId>
			<artifactId>json-simple</artifactId>
			<version>1.1.1</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/com.google.code.gson/gson -->
		<dependency>
			<groupId>com.google.code.gson</groupId>
			<artifactId>gson</artifactId>
			<version>2.9.0</version>
		</dependency>


	</dependencies>
	<build>
		<plugins>
			<plugin>
				<artifactId>maven-eclipse-plugin</artifactId>
				<version>2.9</version>
				<configuration>
					<additionalProjectnatures>
						<projectnature>org.springframework.ide.eclipse.core.springnature</projectnature>
					</additionalProjectnatures>
					<additionalBuildcommands>
						<buildcommand>org.springframework.ide.eclipse.core.springbuilder</buildcommand>
					</additionalBuildcommands>
					<downloadSources>true</downloadSources>
					<downloadJavadocs>true</downloadJavadocs>
				</configuration>
			</plugin>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>2.5.1</version>
				<configuration>
					<source>1.6</source>
					<target>1.6</target>
					<compilerArgument>-Xlint:all</compilerArgument>
					<showWarnings>true</showWarnings>
					<showDeprecation>true</showDeprecation>
				</configuration>
			</plugin>
			<plugin>
				<groupId>org.codehaus.mojo</groupId>
				<artifactId>exec-maven-plugin</artifactId>
				<version>1.2.1</version>
				<configuration>
					<mainClass>org.test.int1.Main</mainClass>
				</configuration>
			</plugin>
		</plugins>
	</build>
</project>

```

- vo
```java
----------------------------------------------UserInfoRequestVO.java----------------------------------------------
package com.itwillbs.fintech.vo;

public class UserInfoRequestVO {
	private String access_token;
	private String user_seq_no;
	public String getAccess_token() {
		return access_token;
	}
	public void setAccess_token(String access_token) {
		this.access_token = access_token;
	}
	public String getUser_seq_no() {
		return user_seq_no;
	}
	public void setUser_seq_no(String user_seq_no) {
		this.user_seq_no = user_seq_no;
	}
	
}

----------------------------------------------UserInfoResponseVO.java----------------------------------------------
package com.itwillbs.fintech.vo;

import java.util.List;

// 2.2.1 사용자 정보조회 API 응답 데이터
public class UserInfoResponseVO {
	private String api_tran_id;
	private String api_tran_dtm;
	private String rsp_code;
	private String rsp_message;
	private String user_seq_no; // 고객마다 다른 고정값
	private String user_ci; // 고객마다 다른 고정값
	private String user_name;
	private String user_info;
	private String user_gender;
	private String user_cell_no;
	private String user_email;
	private String res_cnt;
	private List<AccountVO> res_list;
	
	
	public String getApi_tran_id() {
		return api_tran_id;
	}
	public void setApi_tran_id(String api_tran_id) {
		this.api_tran_id = api_tran_id;
	}
	public String getApi_tran_dtm() {
		return api_tran_dtm;
	}
	public void setApi_tran_dtm(String api_tran_dtm) {
		this.api_tran_dtm = api_tran_dtm;
	}
	public String getRsp_code() {
		return rsp_code;
	}
	public void setRsp_code(String rsp_code) {
		this.rsp_code = rsp_code;
	}
	public String getRsp_message() {
		return rsp_message;
	}
	public void setRsp_message(String rsp_message) {
		this.rsp_message = rsp_message;
	}
	public String getUser_seq_no() {
		return user_seq_no;
	}
	public void setUser_seq_no(String user_seq_no) {
		this.user_seq_no = user_seq_no;
	}
	public String getUser_ci() {
		return user_ci;
	}
	public void setUser_ci(String user_ci) {
		this.user_ci = user_ci;
	}
	public String getUser_name() {
		return user_name;
	}
	public void setUser_name(String user_name) {
		this.user_name = user_name;
	}
	public String getUser_info() {
		return user_info;
	}
	public void setUser_info(String user_info) {
		this.user_info = user_info;
	}
	public String getUser_gender() {
		return user_gender;
	}
	public void setUser_gender(String user_gender) {
		this.user_gender = user_gender;
	}
	public String getUser_cell_no() {
		return user_cell_no;
	}
	public void setUser_cell_no(String user_cell_no) {
		this.user_cell_no = user_cell_no;
	}
	public String getUser_email() {
		return user_email;
	}
	public void setUser_email(String user_email) {
		this.user_email = user_email;
	}
	public String getRes_cnt() {
		return res_cnt;
	}
	public void setRes_cnt(String res_cnt) {
		this.res_cnt = res_cnt;
	}
	public List<AccountVO> getRes_list() {
		return res_list;
	}
	public void setRes_list(List<AccountVO> res_list) {
		this.res_list = res_list;
	}
	
	
}

----------------------------------------------AccountVO.java----------------------------------------------
package com.itwillbs.fintech.vo;

// 2. 사용자/계좌 관리에서 사용되는 계좌 정보(배열로 전달된 데이터)
public class AccountVO {
	private String fintech_use_num;
    private String account_alias;
    private String bank_code_std;
    private String bank_code_sub;
    private String bank_name;
    private String savings_bank_name;
    private String account_num;
    private String account_num_masked;
    private String account_seq;
    private String account_holder_name;
    private String account_holder_type;
    private String account_type;
    private String inquiry_agree_yn;
    private String inquiry_agree_dtime;
    private String transfer_agree_yn;
    private String transfer_agree_dtime;
    private String account_state;
    
	public String getFintech_use_num() {
		return fintech_use_num;
	}
	public void setFintech_use_num(String fintech_use_num) {
		this.fintech_use_num = fintech_use_num;
	}
	public String getAccount_alias() {
		return account_alias;
	}
	public void setAccount_alias(String account_alias) {
		this.account_alias = account_alias;
	}
	public String getBank_code_std() {
		return bank_code_std;
	}
	public void setBank_code_std(String bank_code_std) {
		this.bank_code_std = bank_code_std;
	}
	public String getBank_code_sub() {
		return bank_code_sub;
	}
	public void setBank_code_sub(String bank_code_sub) {
		this.bank_code_sub = bank_code_sub;
	}
	public String getBank_name() {
		return bank_name;
	}
	public void setBank_name(String bank_name) {
		this.bank_name = bank_name;
	}
	public String getSavings_bank_name() {
		return savings_bank_name;
	}
	public void setSavings_bank_name(String savings_bank_name) {
		this.savings_bank_name = savings_bank_name;
	}
	public String getAccount_num() {
		return account_num;
	}
	public void setAccount_num(String account_num) {
		this.account_num = account_num;
	}
	public String getAccount_num_masked() {
		return account_num_masked;
	}
	public void setAccount_num_masked(String account_num_masked) {
		this.account_num_masked = account_num_masked;
	}
	public String getAccount_seq() {
		return account_seq;
	}
	public void setAccount_seq(String account_seq) {
		this.account_seq = account_seq;
	}
	public String getAccount_holder_name() {
		return account_holder_name;
	}
	public void setAccount_holder_name(String account_holder_name) {
		this.account_holder_name = account_holder_name;
	}
	public String getAccount_holder_type() {
		return account_holder_type;
	}
	public void setAccount_holder_type(String account_holder_type) {
		this.account_holder_type = account_holder_type;
	}
	public String getAccount_type() {
		return account_type;
	}
	public void setAccount_type(String account_type) {
		this.account_type = account_type;
	}
	public String getInquiry_agree_yn() {
		return inquiry_agree_yn;
	}
	public void setInquiry_agree_yn(String inquiry_agree_yn) {
		this.inquiry_agree_yn = inquiry_agree_yn;
	}
	public String getInquiry_agree_dtime() {
		return inquiry_agree_dtime;
	}
	public void setInquiry_agree_dtime(String inquiry_agree_dtime) {
		this.inquiry_agree_dtime = inquiry_agree_dtime;
	}
	public String getTransfer_agree_yn() {
		return transfer_agree_yn;
	}
	public void setTransfer_agree_yn(String transfer_agree_yn) {
		this.transfer_agree_yn = transfer_agree_yn;
	}
	public String getTransfer_agree_dtime() {
		return transfer_agree_dtime;
	}
	public void setTransfer_agree_dtime(String transfer_agree_dtime) {
		this.transfer_agree_dtime = transfer_agree_dtime;
	}
	public String getAccount_state() {
		return account_state;
	}
	public void setAccount_state(String account_state) {
		this.account_state = account_state;
	}
    
    
}

----------------------------------------------AccountSearchRequestVO.java----------------------------------------------
package com.itwillbs.fintech.vo;

public class AccountSearchRequestVO {
    private String access_token;
    private String user_seq_no;
    private String include_cancel_yn;
    private String sort_order;
    private String model;
	public String getAccess_token() {
		return access_token;
	}
	public void setAccess_token(String access_token) {
		this.access_token = access_token;
	}
	public String getUser_seq_no() {
		return user_seq_no;
	}
	public void setUser_seq_no(String user_seq_no) {
		this.user_seq_no = user_seq_no;
	}
	public String getInclude_cancel_yn() {
		return include_cancel_yn;
	}
	public void setInclude_cancel_yn(String include_cancel_yn) {
		this.include_cancel_yn = include_cancel_yn;
	}
	public String getSort_order() {
		return sort_order;
	}
	public void setSort_order(String sort_order) {
		this.sort_order = sort_order;
	}
	public String getModel() {
		return model;
	}
	public void setModel(String model) {
		this.model = model;
	}
    
    
}


----------------------------------------------AccountSearchResponseVO.java----------------------------------------------

package com.itwillbs.fintech.vo;

import java.util.List;

public class AccountSearchResponseVO {
	private String api_tran_id;
	private String rsp_code;
	private String rsp_message;
	private String api_tran_dtm;
	private String user_name;
	private String res_cnt;
	private List<AccountVO> res_list;
	public String getApi_tran_id() {
		return api_tran_id;
	}
	public void setApi_tran_id(String api_tran_id) {
		this.api_tran_id = api_tran_id;
	}
	public String getRsp_code() {
		return rsp_code;
	}
	public void setRsp_code(String rsp_code) {
		this.rsp_code = rsp_code;
	}
	public String getRsp_message() {
		return rsp_message;
	}
	public void setRsp_message(String rsp_message) {
		this.rsp_message = rsp_message;
	}
	public String getApi_tran_dtm() {
		return api_tran_dtm;
	}
	public void setApi_tran_dtm(String api_tran_dtm) {
		this.api_tran_dtm = api_tran_dtm;
	}
	public String getUser_name() {
		return user_name;
	}
	public void setUser_name(String user_name) {
		this.user_name = user_name;
	}
	public String getRes_cnt() {
		return res_cnt;
	}
	public void setRes_cnt(String res_cnt) {
		this.res_cnt = res_cnt;
	}
	public List<AccountVO> getRes_list() {
		return res_list;
	}
	public void setRes_list(List<AccountVO> res_list) {
		this.res_list = res_list;
	}
	
	
}
```

- Controller
```java
----------------------------------------------OpenBankingController.java----------------------------------------------
```

- Service
```java
----------------------------------------------OpenBankingService.java----------------------------------------------
----------------------------------------------OpenBankingSApiClient.java----------------------------------------------
```




```jsp
----------------------------------------------bank_main.jsp----------------------------------------------
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
	
	
	<form method="get" action="userInfo" enctype="multipart/form-data">
			<%-- 필요 파라미터는 입력데이터 없이 hidden 속성으로 --%>
			<input type="hidden" name="access_token" value="${responseToken.access_token }">
			<input type="hidden" name="user_seq_no" value="${responseToken.user_seq_no }">
			<input type="submit" value="사용자정보조회">
		</form>
</body>
</html>

----------------------------------------------user_info.jsp----------------------------------------------
```
