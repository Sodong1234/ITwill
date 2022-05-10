# [오전수업] JAVA 43차
## 간단한 계산기 제작

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical"
    android:padding="10dp">

    <EditText
        android:id="@+id/edit1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="30sp"
        android:hint="숫자1"
        android:inputType="number"/>

    <EditText
        android:id="@+id/edit2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="30sp"
        android:hint="숫자2"
        android:inputType="number"/>

    <Button
        android:id="@+id/btnAdd"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="더하기"
        android:textSize="30sp"/>

    <Button
        android:id="@+id/btnSub"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="빼기"
        android:textSize="30sp"/>

    <Button
        android:id="@+id/btnMul"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="곱하기"
        android:textSize="30sp"/>

    <Button
        android:id="@+id/btnDiv"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="나누기"
        android:textSize="30sp"/>

    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="계산결과 : "
        android:textSize="30sp"
        android:id="@+id/tvResult"
        android:textColor="#FF0000"/>

</LinearLayout>
```
```java
package com.example.and0510_basicwidget_simplecalculator;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // 위젯 ID 가져와서 객체 연결하기
        EditText edit1 = findViewById(R.id.edit1);
        EditText edit2 = findViewById(R.id.edit2);

        Button btnAdd = findViewById(R.id.btnAdd);
        Button btnSub = findViewById(R.id.btnSub);
        Button btnMul = findViewById(R.id.btnMul);
        Button btnDiv = findViewById(R.id.btnDiv);

        TextView tvResult = findViewById(R.id.tvResult);

        // 4개의 버튼에 리스너(OnClickListener) 연결하여 이벤트 처리
       View.OnClickListener btnListener = new View.OnClickListener() {
           @Override
           public void onClick(View view) {
               // EditText(edit1, edit2)에 입력된 숫자 2개 가져와서 정수로 변환
               // 연산하여 결과를 TextView(tvResult)에 출력
               // 단, 숫자가 입력되지 않았을 경우 오류 메세지 출력 및 커서 요청
               if (edit1.length() == 0) {
                   Toast.makeText(MainActivity.this, "숫자1 입력 필수!", Toast.LENGTH_SHORT).show();
                   edit1.requestFocus();
                   return;
               } else if (edit2.length() == 0) {
                   Toast.makeText(MainActivity.this, "숫자2 입력 필수!", Toast.LENGTH_SHORT).show();
                   edit2.requestFocus();
                   return;
               }

               int num1 = Integer.parseInt(edit1.getText().toString());
               int num2 = Integer.parseInt(edit2.getText().toString());

               // if문으로 버튼 판별
//               if(view.getId() == R.id.btnAdd) {
//                   Toast.makeText(MainActivity.this, "더하기버튼 클릭!", Toast.LENGTH_SHORT).show();
//               } else if(view.getId() == R.id.btnSub) {
//                   Toast.makeText(MainActivity.this, "빼기버튼 클릭!", Toast.LENGTH_SHORT).show();
//               }

               // 연산 결과를 저장할 변수
               int result = 0;
               // switch - case 문으로 버튼 판별
               switch (view.getId()) {
                   case R.id.btnAdd:
                       result = num1 + num2;
                       break;
                   case R.id.btnSub:
                       result = num1 - num2;
                       break;
                   case R.id.btnMul:
                       result = num1 * num2;
                       break;
                   case R.id.btnDiv:

                       // 나눗셈의 경우 두번째 피연산자가 0이면 ArithmeticException 발생
                       if (num2 == 0) {
                           Toast.makeText(MainActivity.this, "나눗셈에 0 사용 금지", Toast.LENGTH_SHORT).show();;
                           edit2.requestFocus();
                           tvResult.setText("계산 결과 : 오류 발생!");
                           return;
                       }

                       result = num1 / num2;
                       break;
               }

               tvResult.setText("계산결과 : " + result);

           }
       };

      btnAdd.setOnClickListener(btnListener);
      btnSub.setOnClickListener(btnListener);
      btnMul.setOnClickListener(btnListener);
      btnDiv.setOnClickListener(btnListener);

    }




}
```

---

## 체크박스

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical">

    <CheckBox
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/cbAndroid"
        android:text="안드로이드폰"
        android:textSize="30sp"/>

    <CheckBox
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/cbIPhone"
        android:text="아이폰"
        android:textSize="30sp"/>

    <CheckBox
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/cbWindows"
        android:text="윈도우폰"
        android:textSize="30sp"/>

    <CheckBox
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/cbAll"
        android:text="전체 선택"
        android:textSize="30sp"/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="확인"
        android:id="@+id/btnMix"/>
</LinearLayout>
```
```java
package com.example.and0510_basicwidget_checkbox;

import androidx.appcompat.app.AppCompatActivity;

import android.annotation.SuppressLint;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.CompoundButton;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    CheckBox cbAndroid, cbIPhone, cbWindows, cbAll;
    Button btnMix;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

       cbAndroid = findViewById(R.id.cbAndroid);
       cbIPhone = findViewById(R.id.cbIPhone);
       cbWindows = findViewById(R.id.cbWindows);
       cbAll = findViewById(R.id.cbAll);

       btnMix = findViewById(R.id.btnMix);

//       // 체크박스 1개를 대상으로 체크 상태 변화를 감지하는 리스너 연결
//        cbAndroid.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
//            @Override
//            public void onCheckedChanged(CompoundButton compoundButton, boolean isChecked) {
//                if(isChecked) {
//                    Toast.makeText(MainActivity.this, "안드로이드폰 체크됨!", Toast.LENGTH_SHORT).show();
//                } else {
//                    Toast.makeText(MainActivity.this, "안드로이드폰 체크해제됨!", Toast.LENGTH_SHORT).show();
//                }
//
//            }
//        });

        CompoundButton.OnCheckedChangeListener cbListener= new CompoundButton.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(CompoundButton compoundButton, boolean isChecked) {
//                if(isChecked) {
//                    Toast.makeText(MainActivity.this, compoundButton.getText() + "체크됨", Toast.LENGTH_SHORT).show();
//                } else {
//                Toast.makeText(MainActivity.this, compoundButton.getText() + "체크 해제됨", Toast.LENGTH_SHORT).show();
//            }

                // 만약, 현재 리스너를 호출한 체크박스가 '전체 선택' 일 경우
                // 해당 체크박스 상태가 체크 상태이면 세 가지 체크박스를 모두 체크하고,
                //        ""         체크 해제 상태이면      ""          체크해제

                if(compoundButton.getId() == R.id.cbAll) {

                    CheckBox[] checkBoxes = {cbAndroid, cbIPhone, cbWindows};
                    for(CheckBox ch : checkBoxes) {
                        ch.setChecked(isChecked);
                    }

                    cbAndroid.setChecked(isChecked);
                    cbIPhone.setChecked(isChecked);
                    cbWindows.setChecked(isChecked);

//                    if (isChecked) { // 전체 체크
//                        cbAndroid.setChecked(true);
//                        cbIPhone.setChecked(true);
//                        cbWindows.setChecked(true);
//                    } else { // 전체해제
//                        cbAndroid.setChecked(false);
//                        cbIPhone.setChecked(false);
//                        cbWindows.setChecked(false);
//                    }
                }
            }
        };

        cbAndroid.setOnCheckedChangeListener(cbListener);
        cbIPhone.setOnCheckedChangeListener(cbListener);
        cbWindows.setOnCheckedChangeListener(cbListener);
        cbAll.setOnCheckedChangeListener(cbListener);


        // 1. Button을 하나 생성
        // 2. Button 클릭 시 현재 체크되어 있는 항목의 텍스트를 결합하여 출력
        // ex) 안드로이드폰과 아이폰 체크 시 : "안드로이드폰 / 아이폰" 출력

        btnMix.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {

                String items = ""; // 항목을 묶어서 저장할 변수
//                if(cbAndroid.isChecked()) {
//                    items += "/ " + cbAndroid.getText();
//                }
//                if(cbIPhone.isChecked()) {
//                    items += "/ " + cbIPhone.getText();
//                }
//                if(cbWindows.isChecked()) {
//                    items += "/ " + cbWindows.getText();
//                }

//                items = items.substring(3);
                CheckBox[] cbs = {cbAndroid, cbIPhone, cbWindows};
                for(CheckBox cb : cbs) {
                    if(cb.isChecked()) items += cb.getText() + " / ";
                }
                items = items.substring(0, items.length()-3);

                Toast.makeText(MainActivity.this, "선택된 항목 : " + items, Toast.LENGTH_SHORT).show();
            }
        });



    }


}
```

---

# [오후수업] JSP 62차
