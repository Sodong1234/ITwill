# [오전수업] JAVA 63차

## Implict_intent

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
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/btnDial"
        android:text="전화걸기"
        android:textSize="30sp"
        android:onClick="btnClicked"/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/btnWeb"
        android:text="홈페이지 열기"
        android:textSize="30sp"
        android:onClick="btnClicked"/>
    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/btnGoogle"
        android:text="구글맵 열기"
        android:textSize="30sp"
        android:onClick="btnClicked"/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/btnSearch"
        android:text="구글 검색하기"
        android:textSize="30sp"
        android:onClick="btnClicked"/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/btnSms"
        android:text="문자 보내기"
        android:textSize="30sp"
        android:onClick="btnClicked"/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/btnPhoto"
        android:text="사진 찍기"
        android:textSize="30sp"
        android:onClick="btnClicked"/>

</LinearLayout>
```
```java
package com.example.and0628_implict_intent;

import androidx.appcompat.app.AppCompatActivity;

import android.app.SearchManager;
import android.content.Intent;
import android.net.Uri;
import android.os.Bundle;
import android.provider.MediaStore;
import android.view.View;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);


    }

    public void btnClicked(View v) {


        Uri uri = null;
        Intent intent = null;

        switch (v.getId()) {
            case R.id.btnDial: // 전화걸기 버튼일 경우
                // android.net.Uri 객체를 가져오기 위해 Uri.parse() 메서드 호출
                // => 전화번호 입력을 위한 파라미터로 "tel:전화번호" 형식의 문자열을 전달
                uri = Uri.parse("tel:01012345678");
                intent = new Intent(Intent.ACTION_DIAL, uri);
                startActivity(intent);
                break;
            case R.id.btnWeb: // 홈페이지 열기 버튼일 경우
                // 웹브라우저 실행을 위해 Intent.ACTION_VIEW 액션 사용하고
                // Uri 객체에 "접속할 웹 주소(URL)" 문자열 전달
                uri = Uri.parse("http://www.itwillbs.co.kr");
                intent = new Intent(Intent.ACTION_VIEW, uri);
                startActivity(intent);
                break;
            case R.id.btnGoogle: // 구글맵 열기 버튼일 경우
                // 35.1537176,129.057782
                double latitude = 35.1537176; // 위도
                double longitude = 129.057782; // 경도
                
                uri = Uri.parse("http://maps.google.com/maps?q=" + latitude + "," + longitude);
                intent = new Intent(Intent.ACTION_VIEW, uri);
                startActivity(intent);
                break;
            case R.id.btnSearch: // 구글 검색 버튼일 경우
                // Intent 객체 생성 시 ACTION_WEB_SEARCH 상수 전달 ( Uri 객체 없음)
                intent = new Intent(Intent.ACTION_WEB_SEARCH);
                // 검색을 위한 키값은 SEarchManager.QUERY 상수 사용
                intent.putExtra(SearchManager.QUERY, "아이티윌");
                startActivity(intent);

                break;
            case R.id.btnSms: // 문자 보내기 버튼일 경우
                // Intent 객체 생성 시 ACTION_SENDTO 상수 전달 (Uri 객체 없음)
                intent = new Intent(Intent.ACTION_SENDTO);

                intent.putExtra("sms_body", "안녕하세요.");
                uri = Uri.parse("smsto:01012345678");
                intent.setData(uri);
                startActivity(intent);
                break;
            case R.id.btnPhoto: // 사진 찍기 버튼일 경우
                // intent 객체 생성 시 MediaStore.ACTION_IMAGE_CAPTURE 상수 전달 (uri 객체 없음)
                intent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
                startActivity(intent);
                break;

        }

    }
}
```
