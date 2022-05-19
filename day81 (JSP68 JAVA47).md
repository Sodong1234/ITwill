# [오전수업] JSP 68차
> 테스트 실시

--- 

# [오후수업] JAVA 47차

## layout

```xml
-------------------------------------------------------activity_main.xml-------------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/button"
        android:text="Button"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/button2"
        android:text="Button"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/button3"
        android:text="Button"/>



</LinearLayout>
-------------------------------------------------------AndroidManifest.xml-------------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.and0519_layout_linearlayout">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.And0519_Layout_LinearLayout">


        <!-- 어플리케이션 시작 시 표시할 액티비티(프로그램 시작점) 지정 방법 -->
        <!-- 표시할 액티비티를 MainActivity 클래스로 지정 -->
        
                <activity
                    android:name=".MainActivity"
                    android:exported="true">
        <!-- 표시할 액티비티를 LayoutTestActivity 클래스로 변경 -->
<!--                <activity-->
<!--                    android:name=".layoutTestActivity"-->
<!--                    android:exported="true">-->
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```
```java
-------------------------------------------------------MainActivity.java-------------------------------------------------------
package com.example.and0519_layout_linearlayout;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.ViewGroup;
import android.widget.Button;
import android.widget.LinearLayout;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // layout 폴더 내의 activity_name.xml 파일을 현재 액티비티에서 표시
//        setContentView(R.layout.activity_main);
        // => 만약, 이 코드를 실행하지 않으면 activity_main.xml 파일의 레이아웃 형태가 안 보임

        /*
        * [ 자바 코드를 사용하여 레이아웃 직접 생성하기 ]
        *
        * 1. LinearLayout 객체를 생성
        *    => 현재 액티비티 클래스 객체를 전달해야하므로 레퍼런스 this를 파라미터로 전달
        * 2. LinearLayout 객체의 setXXX() 메서드를 호출하여 속성 설정 가능
        *    => 필수 속성 중 orientation 속성 변경 위해 setOrientation() 메서드 호출
        *       (LinearLayout 클래스의 상수를 사용하여 VERTICAL or HORIZONTAL 지정)
        *
        * --------------------------------------------------------------------------
        * [ 레이아웃 내에서 표시할 위젯 생성하기 ]
        * 3. 생성할 위젯의 객체 생성
        * 4. 위젯 속성 변경
        * --------------------------------------------------------------------------
        * [ 생성된 위젯과 레이아웃을 각각 표시하기 ]
        * 5. 생성된 레이아웃에 생성된 위젯 표시(addView())
        * 6. 레이아웃을 액티비티에 표시(setContentView())
        *
        *
        */
        LinearLayout layout = new LinearLayout(this);
        // => 현재 액티비티 객체 (AppCompatActivity 클래스를 상속받은 객체)를 파라미터로 전달
        layout.setOrientation(LinearLayout.VERTICAL);   // orientation 속성을 VERTICAL로 변경

        // 버튼 위젯 생성을 위한 Button 객체 생성
        Button btn = new Button(this);

        // 버튼 위젯 속성 설정하기 위해 Button 객체의 setXXX() 메서드 호출
        // 1. 버튼 텍스트 변경
        btn.setText("버튼");

        // 2. 버튼 텍스트 사이즈 변경
        btn.setTextSize(20);

        // 3. 버튼 크기 변경 위해 layoutParams 객체를 생성하여 WIDTH, HEIGHT 값 설정
        // => LayoutParams.XXX 상수를 활용하여 MATCH_PARENT 또는 WRAP_CONTENT 값 설정 가능
        //    (LinearLayout.LayoutParams.MATCH_PARENT 와 ViewGroup.LayoutParams.MATCH_PARENT 동일)
        LinearLayout.LayoutParams params = new LinearLayout.LayoutParams(
                LinearLayout.LayoutParams.MATCH_PARENT,
                LinearLayout.LayoutParams.WRAP_CONTENT
        );

        // 4. 지정된 레이아웃 크기를 담은 LayoutParams 객체를
        //    Button 객체의 setLayoutParams) 메서드 호출하여 파라미터로 전달
        btn.setLayoutParams(params);

        // 5. 생성된 레이아웃 객체의 addView() 메서드를 호출하여 Button 객체를 파라미터로 전달
        layout.addView(btn);

        // 6. setContentView() 메서드 호출하여 생성된 레이아웃 객체를 파라미터로 전달하여 표시
        setContentView(layout);

        // ============================================================================
        // 버튼 객체 1개(btn2, "버튼2") 를 생성하여 레이아웃에 추가하기
        Button btn2 = new Button(this);
        btn2.setText("버튼2");
        btn2.setTextSize(20);
        LinearLayout.LayoutParams params2 = new LinearLayout.LayoutParams(
                LinearLayout.LayoutParams.MATCH_PARENT,
                LinearLayout.LayoutParams.WRAP_CONTENT
        );

        btn2.setLayoutParams(params2);
        layout.addView(btn2);


    }
}
-------------------------------------------------------layoutTestActivity.java-------------------------------------------------------
package com.example.and0519_layout_linearlayout;


import android.os.Bundle;

import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;


// 화면을 구성하는 액티비티 클래스는 반드시 AppCompatActivity 클래스를 상속받아 정의해야한다
// => onCreate() 메서드를 오버라이딩 하여 초기 코드 작성
public class layoutTestActivity extends AppCompatActivity {

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        // 표시할 레이아웃 파일을 setContentView() 메서드를 호출하여 지정
        setContentView(R.layout.activity_main);
    }
}

```

## gravity 속성과 layout_gravity속성
> 위에서 생성한 프로젝트의 /res/layout 폴더에 파일만 생성하였음.
```xml
-------------------------------------------------------gravity.xml-------------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <!-- gravity 속성과 layout_gravity 속성
    1. gravity 속성 : 대상 뷰의 "내부" 정렬 설정
        - 속성값에 left, right 등 위치 전달을 통해 정렬 설정
          또한, 복수개의 속성 설정 시 설정값을 | 기호로 구분
        - 별도의 gravity 속성을 지정하지 않으면 기본값은 center(가운데정렬)
        - top, bottom : 수직 방향 지정(기본값 top)
        - left, right : 수평 방향 지정(기본값 left)
          => top 또는 bottom 이나 left 또는 right 중 하나만 지정 시 기본값 동작
        - center_vertical : 수직 방향 중앙 정렬
        - center_horizontal : 수평 방향 중앙 정렬
        - center : 한 가운데(center_vertical|center_horizontal 과 동일)
        ex) left|center_vertical : 수평방향 좌측, 수직방향 중앙 (= 좌측 중앙)
            right|bottom : 수평 방향 우측, 수직 방향 하단(우측 하단)

    -->
    <Button
        android:layout_width="300dp"
        android:layout_height="100dp"
        android:text="GRAVITY_LEFT"
        android:gravity="left|center_vertical"/>

    <Button
        android:layout_width="300dp"
        android:layout_height="100dp"
        android:text="GRAVITY_CENTER"
        android:gravity="center_vertical|center_horizontal"/>

    <Button
        android:layout_width="300dp"
        android:layout_height="100dp"
        android:text="GRAVITY_RIGHT"
        android:gravity="right|bottom"/>

    <!--
    2. layout_gravity 속성 : 부모로부터 자신의 우치ㅣ 정렬
    => Linearlayout의 기본 정렬 위치 : 좌측 상단(left|top)
    => layout_gravity 속성 위치 변경 시 내부 위젯이 전체 이동됨
    -->

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="LEFT"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="CENTER"
        android:layout_gravity="center_horizontal"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="RIGHT"
        android:layout_gravity="right"/>

    <!--
    주의! 레이아웃의 gravity 속성을 지정하면 레이아웃 내의 위젯 위치가 변경됨
    -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        android:gravity="right|bottom">

        <!--
        주의! 레이아웃 내의 위젯의 layout_gravity 속성을 지정하면
        레이아웃 내의 위젯 위치가 변경됨 (단독 동작)
        -->
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="버튼1"
            android:layout_gravity="left|top"/>

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="버튼2"/>

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="버튼3"/>
    </LinearLayout>



</LinearLayout>

```

- 연습문제
```xml
-------------------------------------------------------gravity_test.xml-------------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <Button
        android:layout_width="150dp"
        android:layout_height="150dp"
        android:text="버튼1"
        android:gravity="right|top"
        android:layout_gravity="center"/>

    <Button
        android:layout_width="150dp"
        android:layout_height="150dp"
        android:gravity="left|center_vertical"
        android:text="버튼2"/>

    <Button
        android:layout_width="150dp"
        android:layout_height="150dp"
        android:text="버튼3"
        android:gravity="right|bottom"
        android:layout_gravity="right"/>
</LinearLayout>
```

## layout weight
```xml
-------------------------------------------------------layout_weight.xml-------------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <!--
    layout_weight 속성 : 특정 위젯 또는 레이아웃의 크기 지정 시 여유 공간에 대한 할당 비율 설정
    ex) 수평 배치된 버튼 2개의 layout_width 속성이 wrap_content 이고
        1개 버튼의 layout_weight 속성값이 1일 경우
        다른 버튼을 배치하고 남은 공간 전체(1만큼)를 자신에게 할당함
    -->

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <!-- 레이아웃 내부 위젯의 weight 값을 모두 1로 설정(1:1 비율) -->
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:backgroundTint="#FFFF88"
            android:text="버튼1"
            android:layout_weight="1"/>

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:backgroundTint="#88FFFF"
            android:text="버튼2"
            android:layout_weight="1"/>

    </LinearLayout>


    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <!--
        레이아웃 내부 위젯 중 하나의 weight 값을 1로 설정
        -->
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="버튼1"
            android:backgroundTint="#FFFF88"
            android:layout_weight="1"/>

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="버튼2"
            android:backgroundTint="#88FFFF"/>

    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">
        <!--
        레이아웃 내부 위젯 중 하나의 weight 값은 1로 설정하고
        다른 하나의 weight 값은 2로 설정할 경우
        => 총 weight 값의 합이 3이므로 여유공간의 1/3 과 2/3 크기를
           각각의 위젯에 할당하여 더하게 됨
        => 주의! 전체 공간을 1/3과 2/3로 분할하는 것이 아닌
           여유 공간을 분할하여 할당하는 것이므로 위젯 비율은 달라짐
        -->
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="버튼1"
            android:backgroundTint="#FFFF88"
            android:layout_weight="1"/>

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="버튼2"
            android:backgroundTint="#88FFFF"
            android:layout_weight="2"/>

    </LinearLayout>

        <!--
        레이아웃 내의 위젯 크기를 원하는 비율대로 분할 하기 위해서는
        분할하려는 방향의 크기를 0dp 로 지정하고,
        layout_weight 속성값으로 실제 분할 비율을 지정해야한다!
        -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <Button
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:text="버튼1"
            android:backgroundTint="#FFFF88"
            android:layout_weight="1"/>

        <Button
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:text="버튼2"
            android:backgroundTint="#88FFFF"
            android:layout_weight="2"/>

    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <!--
        리니어레이아웃의 배치 방향을 vertical 로 지정하는 경우
        layout_height 속성값을 0dp 로 변경하고
        layout_weight 속성값으로 실제 분할 비율을 지정해야한다!
        -->
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="버튼1"
            android:backgroundTint="#FFFF88"
            android:layout_weight="1"/>

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="버튼2"
            android:backgroundTint="#88FFFF"
            android:layout_weight="2"/>

    </LinearLayout>


</LinearLayout>
```
