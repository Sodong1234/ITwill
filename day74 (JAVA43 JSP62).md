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


---

# [오후수업] JSP 62차
