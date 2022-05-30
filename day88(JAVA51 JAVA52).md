# JAVA 51, 52차
## AdWidget_SlidingDrawer
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
        android:text="메뉴 화면입니다. (서랍 밖)"
        android:textSize="30sp"/>

    <SlidingDrawer
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:handle="@id/btnHandle"
        android:content="@id/layoutContent">

        <Button
            android:id="@+id/btnHandle"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="서랍손잡이"
            android:textSize="30sp" />

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:orientation="vertical"
            android:id="@+id/layoutContent"
            android:background="#88FFFF">

            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:text="서랍 안 입니다."
                android:textSize="30sp"/>

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:orientation="vertical">

                <Button
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="장바구니"
                    android:textSize="30sp"/>
                <Button
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="즉시주문"
                    android:textSize="30sp"/>

                <!-- ================================================ -->
                <!-- 서랍 내부에 또 다른 서랍 추가 -->
                <!-- handle : Button(btnHandle2, 안쪽 서랍 손잡이) -->
                <!-- content : LinearLayout(layoutContent2) -->
                <SlidingDrawer
                    android:layout_width="match_parent"
                    android:layout_height="match_parent"
                    android:handle="@+id/btnHandle2"
                    android:content="@+id/layoutContent2">

                    <Button
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:id="@+id/btnHandle2"
                        android:text="안쪽 서랍 손잡이"
                        android:textSize="20sp"/>

                    <LinearLayout
                        android:layout_width="match_parent"
                        android:layout_height="match_parent"
                        android:orientation="vertical"
                        android:id="@+id/layoutContent2"
                        android:background="#FF88FF">

                        <ImageView
                            android:layout_width="wrap_content"
                            android:layout_height="wrap_content"
                            android:src="@drawable/dog"></ImageView>

                    </LinearLayout>


                </SlidingDrawer>



            </LinearLayout>

        </LinearLayout>
    </SlidingDrawer>
</LinearLayout>
```

## AdWidget_ViewFlipper

```xml
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

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <Button
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="이전화면"
            android:textSize="20sp"
            android:id="@+id/btnPrev"/>

        <Button
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="다음화면"
            android:textSize="20sp"
            android:id="@+id/btnNext"/>

    </LinearLayout>
    <!-- 화면 전환을 통해 서로 다른 위젯 또는 레이아웃을 보여주는 viewFlipper -->
    <ViewFlipper
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/viewFlipper">
        <!--
        화면 전환으로 표시할 3개의 레이아웃 배치
        => 화면 전환을 위한 기능으로 내부 위젯 또는 레이아웃의 ID는 불필요
        -->

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:background="#FF8800" />

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:background="#00FF88"/>

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:background="#FF0088"/>


        </ViewFlipper>
    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:orientation="vertical"
        android:layout_weight="1">

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:orientation="horizontal">

            <Button
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:text="사진보기 시작"
                android:textSize="20sp"
                android:id="@+id/btnStart"/>

            <Button
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:text="사진보기 정지"
                android:textSize="20sp"
                android:id="@+id/btnStop"/>
            </LinearLayout>

        <ViewFlipper
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:id="@+id/viewFlipper2">

            <ImageView
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:src="@drawable/mov25"/>

            <ImageView
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:src="@drawable/mov27"/>

            <ImageView
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:src="@drawable/mov29"/>

        </ViewFlipper>


    </LinearLayout>

</LinearLayout>
```
```java
package com.example.and0530_adwidget_viewflipper;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.widget.Button;
import android.widget.ViewFlipper;

public class MainActivity extends AppCompatActivity {

    Button btnPrev, btnNext, btnStart, btnStop;
    ViewFlipper viewFlipper, viewFlipper2;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        btnPrev = findViewById(R.id.btnPrev);
        btnNext = findViewById(R.id.btnNext);
        btnStart = findViewById(R.id.btnStart);
        btnStop = findViewById(R.id.btnStop);
        viewFlipper = findViewById(R.id.viewFlipper);
        viewFlipper2 = findViewById(R.id.viewFlipper2);

        btnPrev.setOnClickListener(view -> {
           viewFlipper.showPrevious();
        });

        btnNext.setOnClickListener(view -> viewFlipper.showNext());

        btnStart.setOnClickListener(view -> {
            // 자동으로 1초마다 다음 위젯으로 전환하기 위해서는
            // ViewFlipper 객체의 startFlipping() 메서드를 호출하여 전환을 시작하고
            // setFlipInterval() 메서드를 호출하여 전환 간격 지정 (단위 : ms)
            viewFlipper2.startFlipping();
            viewFlipper2.setFlipInterval(500);
        });

        btnStop.setOnClickListener(view -> {
            viewFlipper2.stopFlipping();
        });
    }
}
```
