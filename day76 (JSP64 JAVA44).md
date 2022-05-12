# [오전수업] JSP 64차
> - 유스케이스 다이어그램, 클래스 다이어그램, 시퀀스 다이어그램 그리는 법 교육.
> - JSP 63차에서 실시한 스프링 설정작업 추가 실시(커밋해놓음).
---

# [오후수업] JAVA 44차

## 라디오버튼
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical">

<RadioButton
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:text="라디오버튼"
    android:textSize="20sp"/>

<RadioButton
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:text="라디오버튼2"
    android:textSize="20sp"/>
    <!-- RadioButton 그룹화를 위해서 RadioGroup 태그 사용하여 묶기 -->
    <RadioGroup
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/rGroup">

        <RadioButton
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="식사 가능"
            android:textSize="20sp"
            android:checked="true"
            android:id="@+id/rb1"/>

        <RadioButton
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="식사 불가능"
            android:textSize="20sp"
            android:id="@+id/rb2"/>
    </RadioGroup>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/btnOk"
        android:text="확인"
        android:textSize="30sp"/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/btnReset"
        android:text="초기화"
        android:textSize="30sp"/>



</LinearLayout>
```
```java
package com.example.and0512_basicwidget_radiobutton;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.RadioButton;
import android.widget.RadioGroup;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        RadioGroup rGroup = findViewById(R.id.rGroup);
        RadioButton rb1 = findViewById(R.id.rb1);
        RadioButton rb2 = findViewById(R.id.rb2);

        Button btnOk = findViewById(R.id.btnOk);
        Button btnReset = findViewById(R.id.btnReset);

        // 복수개의 RadioButton 이 하나의 RadioGroup으로 묶여 있을 경우
        // 각각의 리스너를 별도로 연결하지 않고 RadioGroup 객체에만 연결하면 모두 이벤트 처리 가능
        // => CheckBox와 동일한 OnCheckedChangeListener 리스너 사용
//        rGroup.setOnCheckedChangeListener(new RadioGroup.OnCheckedChangeListener() {
//            @Override
//            public void onCheckedChanged(RadioGroup radioGroup, int checkedId) {
//                // 파라미터 int checkedId 에는 선택된 RadioButton 위젯의 ID값이 전달됨
//                // => switch-case 문을 활용하여 Toast Message 출력!
//                switch (checkedId){
//                    case R.id.rb1:
//                        Toast.makeText(MainActivity.this, "식사 가능!", Toast.LENGTH_SHORT).show();
//                        break;
//                    case R.id.rb2:
//                        Toast.makeText(MainActivity.this, "식사 불가능!", Toast.LENGTH_SHORT).show();
//                        break;
//                }
//
//
//            }
//        });


        // 확인 버튼을 클릭하면
        // 위와 같이 선택한 라디오 버튼에 따라 "식사 가능" 또는 "식사 불가능" 을
        // Toast Message 로 출력!
        btnOk.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                switch (rGroup.getCheckedRadioButtonId()){
                    case R.id.rb1:
                        Toast.makeText(MainActivity.this, "식사 가능!", Toast.LENGTH_SHORT).show();
                        break;
                    case R.id.rb2:
                        Toast.makeText(MainActivity.this, "식사 불가능!", Toast.LENGTH_SHORT).show();
                        break;
                }
            }
        });

        // 초기화 버튼 클릭시
        // 선택된 모든 RadioButton 선택 해제 (초기화)
//        btnReset.setOnClickListener(new View.OnClickListener() {
//            @Override
//            public void onClick(View view) {
////                rb1.setChecked(false);
////                rb2.setChecked(false);
//
//                // RadioGroup의 clearCheck() 메서드를 호출하여 그룹 전체를 초기화 하는 방법
//                rGroup.clearCheck();
//            }
//        });


        // 람다식
        btnReset.setOnClickListener(view -> rGroup.clearCheck());


    }
}
```

## 스위치 토글 버튼
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical">

    <Switch
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/sw1"
        android:text="Wi-fi"
        android:textSize="30sp"/>

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/tv"
        android:text="Wi-Fi 상세 옵션 설정"
        android:textSize="20sp"
        android:layout_margin="40dp"
        android:visibility="gone"/>

    <Switch
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/swSub"
        android:text="Wi-Fi boost"
        android:layout_marginLeft="40dp"
        android:visibility="gone"/>

    <Switch
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/sw2"
        android:text="Bluetooth"
        android:textSize="30sp"
        android:checked="true"/>

    <ToggleButton
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Wi-Fi"
        android:id="@+id/toggle1"
        android:textSize="30sp"
        android:checked="true"/>

    <ToggleButton
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/toggle2"
        android:textOff="Bluetooth OFF"
        android:textOn="Bluetooth ON"/>
</LinearLayout>
```
```java
package com.example.and0512_basicwidget_switch_togglebutton;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.CompoundButton;
import android.widget.Switch;
import android.widget.TextView;
import android.widget.Toast;
import android.widget.ToggleButton;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Switch sw1 = findViewById(R.id.sw1);
        TextView tv = findViewById(R.id.tv);
        Switch swSub = findViewById(R.id.swSub);

        ToggleButton toggle1 = findViewById(R.id.toggle1);
        Switch sw2 = findViewById(R.id.sw2);
        ToggleButton toggle2 = findViewById(R.id.toggle2);

        // Switch 이벤트는 CheckBox와 동일한 OnCheckedChangeListener 사용
        sw1.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(CompoundButton compoundButton, boolean isChecked) {
//                if(isChecked) {
////                    Toast.makeText(MainActivity.this, sw1.getText() + "ON", Toast.LENGTH_SHORT).show();
//                    tv.setVisibility(View.VISIBLE);
//                    swSub.setVisibility(View.VISIBLE);
//
//                } else {
////                    Toast.makeText(MainActivity.this, sw1.getText() + "OFF", Toast.LENGTH_SHORT).show();
//                    tv.setVisibility(View.GONE);
//                    swSub.setVisibility(View.GONE);
//                }
                int result = isChecked ? View.VISIBLE : View.GONE;
                tv.setVisibility(result);
                swSub.setVisibility(isChecked ? View.VISIBLE : View.GONE);

            }

        });

        toggle1.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(CompoundButton compoundButton, boolean isChecked) {
                if(isChecked) {
                    Toast.makeText(MainActivity.this, toggle1.getText(), Toast.LENGTH_SHORT).show();
                } else {
                    Toast.makeText(MainActivity.this, toggle1.getText(), Toast.LENGTH_SHORT).show();
                }
            }
        });

        // 람다식
        toggle2.setOnCheckedChangeListener((button, isChecked) -> sw2.setChecked(isChecked));

    }
}

```

## 이미지뷰
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical">

    <!--
    ImageView : 사진 등의 이미지를 표시하는 위젯
    => 기본적으로 scaleType 속성값이 finCenter 로 지정되어 있기 때문에
       중심점을 기준으로 비율에 맞게 이미지를 확대/축소 시킴
    -->
    <ImageView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/ic_launcher"
        android:id="@+id/iv1"/>

    <!--
    ImageButton : 기존 Button 과 형태는 동일하나 상속 구조가 달라 속성 등이 다름
    => 이벤트 처리 과정은 동일함
    => 기본적으로 scaleType 속성값이 center 로 지정되어 있기 때문에
       중심점을 기준으로 원본 크기 그대로 배치함
    -->
    <ImageButton
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/ic_launcher"
        android:id="@+id/iBtn1"/>
</LinearLayout>
```
```java
package com.example.and0512_basicwidget_imageview;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.ImageButton;
import android.widget.ImageView;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        ImageView iv1 = findViewById(R.id.iv1);
        ImageButton iBtn1 = findViewById(R.id.iBtn1);

        // ImageButton 클릭 시 ImageView 이미지 변경
        iBtn1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                iv1.setImageResource(R.drawable.dog);
            }
        });

        iv1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Toast.makeText(MainActivity.this, "ImageView 클릭됨!", Toast.LENGTH_SHORT).show();
            }
        });
        // ImageView를 롱클릭 시 원래 이미지(ic_launcher.png)로 변경
        // 주의! => 만약, LongClick과 Click이 모두 리스너로 등록되어 있을 경우
        //          OnLongClickListener 가 먼저 동작 후 마우스 버튼을 떼면 onClickListener 가 동작함

        iv1.setOnLongClickListener(new View.OnLongClickListener() {
            @Override
            public boolean onLongClick(View view) {
                iv1.setImageResource(R.drawable.ic_launcher);
                return false;
            }
        });

    }
}
```


