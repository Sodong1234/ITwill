# [오전수업] JSP 84차

> 평가원 테스트 실시
---

# [오후수업] JAVA 58차

## Login Dialog 

```xml
-------------------------------------------activity_main.xml-------------------------------------------
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
        android:layout_height="0dp"
        android:layout_weight="1"
        android:orientation="vertical">




    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="로그인"
        android:textSize="20sp"
        android:id="@+id/btnLogin"/>

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/tvId"
        android:text="아이디 : "
        android:textSize="30sp"/>

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/tvPassword"
        android:text="패스워드 : "
        android:textSize="30sp"/>
    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:orientation="vertical">

        <Button
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/btnAddress"
            android:text="주소 입력"
            android:textSize="20sp"/>

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/tvAddress"
            android:text="주소 : "
            android:textSize="30sp"/>

    </LinearLayout>
</LinearLayout>


-------------------------------------------login_dialog.xml-------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/etId"
        android:textSize="30sp"
        android:hint="아이디 입력"/>

    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/etPassword"
        android:textSize="30sp"
        android:hint="패스워드 입력"/>

</LinearLayout>
-------------------------------------------address_dialog.xml-------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="10dp">

    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/etAddress"
        android:textSize="20sp"
        android:hint="주소 입력"
        android:singleLine="true"/>
</LinearLayout>
```

```java
-------------------------------------------MainActivity.java-------------------------------------------
package com.example.and0616_login_dialog;

import androidx.appcompat.app.AlertDialog;
import androidx.appcompat.app.AppCompatActivity;

import android.content.DialogInterface;
import android.os.Bundle;
import android.view.View;
import android.view.ViewGroup;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {
    String dbId = "admin";
    String dbPassword = "1234";

    Button btnLogin, btnAddress;
    TextView tvId, tvPassword, tvAddress;

    EditText etId, etPassword, etAddress;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        btnLogin = findViewById(R.id.btnLogin);
        tvId = findViewById(R.id.tvId);
        tvPassword = findViewById(R.id.tvPassword);

        btnAddress = findViewById(R.id.btnAddress);
        tvAddress = findViewById(R.id.tvAddress);
        
        // 로그인 버튼 클릭 시 OnClickListener 연결
        // => 다이얼로그 생성하여 표시
        //    1) 제목(타이틀) : 로그인 정보 입력
        //    2) 아이콘 : wipmap -> ic_launchar_round


            btnLogin.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View view) {

                    AlertDialog.Builder dialog = new AlertDialog.Builder(MainActivity.this);
                    // => 주의! 현재 구현체 클래스 내이므로 this 대신 액티비티명.this 사용
                    dialog.setTitle("로그인 정보 입력");
                    dialog.setIcon(R.mipmap.ic_launcher_round);
                    
                    // ----------------------------------------------
                    // 다이얼로그 커스터마이징ㅇ르 위한 레이아웃 삽입
                    // => layout 폴더 내의 login_dialog.xml 파일 내용을
                    //    현재 생성된 다이얼로그 컨텐츠로 사용
                    // => 외부 XML 파일을 불러오려면 Inflate 기능을 사용하여 메모리에 로딩 후
                    //    필요한 객체에 전달하여 사용해야함
                    // 1. 레이아웃(XML 파일) 메모리에 로딩하기
                    //    => View.inflate() 메서드를 호출하여 XML 로딩하기
                    //       => 파라미터 : 컨텍스트 객체. 표시한 XML 파일, ViewGroup 객체
                    View loginDialogView = View.inflate(MainActivity.this, R.layout.login_dialog, null);
                    // 2. 레이아웃을 현재 다이얼로그에 표시하기
                    //    => AlertDialog.Builder 객체의 setView() 메서드를 호출하여 레이아웃 표시
                    //          파라미터 : 레이아웃을 담고 있는 View 객체 (loginDialogView)
                    dialog.setView(loginDialogView);
                    // ----------------------------------------------

                    dialog.setNegativeButton("취소", null);

                    // 확인 버튼 클릭 시 다이얼로그 내의 EditText에서 입력된 ID, Password 가져온 후
                    // activity_main.xml의 TextView에 출력하기
                    dialog.setPositiveButton("확인", null);

                    dialog.setPositiveButton("확인", new DialogInterface.OnClickListener() {
                        @Override
                        public void onClick(DialogInterface dialogInterface, int i) {
                            // EditText위젯 ID를 가져오기
//                            etId = findViewById(R.id.etId);
                            // => 주의! 문법적 오류는 발생하지 않지만 해당 객체에 접근하려 할 경우
                            //    NullPointerException 발생하게 된다!ㅁ
//                            Toast.makeText(MainActivity.this, etId.getText().toString(), Toast.LENGTH_SHORT).show();

                            // 현재 액티비티(MainActivity)에 연결된 레이아웃(activity_main.xml)이 아닌
                            // 다른 외부 레이아웃(ex. login_dialog.xml)의 위젯 ID를 연결하려면
                            // 해당 위젯이 포함된 레이아웃을을 담고 있는 View 객체를 통해
                            // findViewById() 메서드를 호출해야한다!
                            // View loginDialogView = View.inflate(MainActivity.this, R.layout.login_dialog, null);

                            etId = loginDialogView.findViewById(R.id.etId);
                            etPassword = loginDialogView.findViewById(R.id.etPassword);

                            // 입력받은 Id, Password 값을 가져와서 저장된 ID, Password와 비교
                            String id = etId.getText().toString();
                            String password = etPassword.getText().toString();

                            // ID, Password 가 모두 일치할 경우
                            if(id.equals(dbId) && password.equals(dbPassword)) {
                                // TextView 위젯에 ID, PAssword 값 출력
                                tvId.setText("아이디 : " + id);
                                tvPassword.setText("패스워드 : " + password);
                                Toast.makeText(MainActivity.this, "로그인 성공!", Toast.LENGTH_SHORT).show();
                            } else {
                                Toast.makeText(MainActivity.this, "로그인 실패!", Toast.LENGTH_SHORT).show();
                            }
                        }
                    });

                    dialog.show();
                }

            });

            
            // 주소입력 버튼에 대한 이벤트 처리
            // 주소입력 버튼 클릭 시 다이얼로그 생성
            // 1) 제목(타이틀) : 주소 정보 입력
            // 2) address_dialog.xml 파일 메모리에 로딩 후 다이얼로그 연결
            // 3) 취소, 확인 버튼 생성
            //    => 확인버튼 이벤트 처리: address_dialog.xml 입력한 데이터를 가져와서
            //                          tvAddress에 표기

        btnAddress.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {

                 AlertDialog.Builder dialog = new AlertDialog.Builder(MainActivity.this);
                dialog.setTitle("주소 정보 입력");

                View addressDialogView = View.inflate(MainActivity.this, R.layout.address_dialog, null);
                dialog.setView(addressDialogView);
                // ----------------------------------------------

                dialog.setNegativeButton("취소", null);

                dialog.setPositiveButton("확인", null);

                dialog.setPositiveButton("확인", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialogInterface, int i) {

                    etAddress = addressDialogView.findViewById(R.id.etAddress);
                    String address = etAddress.getText().toString();
                    tvAddress.setText("주소 : " + address);

                    }
                });

                dialog.show();
            }

        });

        }

    }
```

## Progress Seek Rating
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical">

    <ProgressBar
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/progressBar"
        style="@style/Widget.AppCompat.ProgressBar.Horizontal"
        android:layout_margin="10dp"
        android:progress="50"
        android:secondaryProgress="80"/>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/btnProgressPlus"
            android:text="Progress 증가"
            android:textSize="20sp"/>

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/btnSecondaryProgressPlus"
            android:text="Secondary 증가"
            android:textSize="20sp"/>

    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <Button
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/btn1"
            android:text="원형 프로그레스바 표시"
            android:textSize="20sp"/>

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/btn2"
            android:text="닫기"
            android:textSize="20sp"/>

    </LinearLayout>

</LinearLayout>
```
```java
package com.example.and0616_progress_seek_rating;

import androidx.appcompat.app.AppCompatActivity;

import android.app.ProgressDialog;
import android.os.Bundle;
import android.widget.Button;
import android.widget.ProgressBar;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        ProgressBar progressBar = findViewById(R.id.progressBar);
        Button btnProgressPlus = findViewById(R.id.btnProgressPlus);
        Button btnSecondaryProgressPlus = findViewById(R.id.btnSecondaryProgressPlus);
        Button btn1 = findViewById(R.id.btn1);
        Button btn2 = findViewById(R.id.btn2);


        btnProgressPlus.setOnClickListener(view -> {
           progressBar.setProgress(progressBar.getProgress() + 5);
        });
        btnSecondaryProgressPlus.setOnClickListener(view -> {
            progressBar.setSecondaryProgress(progressBar.getSecondaryProgress() + 5);
        });

        ProgressDialog progressDialog = new ProgressDialog(MainActivity.this);

        btn1.setOnClickListener(view -> {
            progressDialog.setProgressStyle(ProgressDialog.STYLE_SPINNER);
            progressDialog.setMessage("로딩중입니다.");
            progressDialog.show();
        });

        btn2.setOnClickListener(view -> {
            progressDialog.dismiss();   // ProgressDialog 창 닫기
        });
    }
}
```
