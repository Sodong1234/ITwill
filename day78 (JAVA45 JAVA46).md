# [오전수업] JAVA 45차
## Picture Viewer


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
        android:text="선택을 시작하시겠습니까?"
        android:textSize="30sp"/>

    <CheckBox
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="시작함"
        android:textSize="20sp"
        android:id="@+id/checkStart"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="좋아하는 애완동물은?"
        android:textSize="30sp"
        android:visibility="invisible"
        android:id="@+id/tvPet"/>

    <RadioGroup
        android:id="@+id/rGroup"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:visibility="invisible">

        <RadioButton
            android:id="@+id/rbDog"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="강아지"
            android:textSize="20sp" />

        <RadioButton
            android:id="@+id/rbCat"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="고양이"
            android:textSize="20sp"/>

        <RadioButton
            android:id="@+id/rbRabbit"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="토끼"
            android:textSize="20sp"/>

    </RadioGroup>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="선택완료"
        android:textSize="30sp"
        android:visibility="invisible"
        android:id="@+id/btnOk"/>

    <ImageView
        android:id="@+id/ivPet"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:scaleType="fitCenter"
        android:layout_weight="1"
        android:visibility="invisible"/>

    <Button
        android:id="@+id/btnReset"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="초기화"
        android:textSize="30sp"/>

    <Button
        android:id="@+id/btnFinish"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="종료"
        android:textSize="30sp"/>


</LinearLayout>
```
```java
package com.example.and0516_basicwidget_picture_viewer;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.CompoundButton;
import android.widget.ImageView;
import android.widget.RadioGroup;
import android.widget.TextView;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    CheckBox checkStart;
    TextView tvPet;
    RadioGroup rGroup;
    Button btnOk, btnReset, btnFinish;
    ImageView ivPet;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        checkStart = findViewById(R.id.checkStart);
        tvPet = findViewById(R.id.tvPet);
        rGroup = findViewById(R.id.rGroup);
        btnOk = findViewById(R.id.btnOk);
        btnReset = findViewById(R.id.btnReset);
        btnFinish = findViewById(R.id.btnFinish);
        ivPet = findViewById(R.id.ivPet);

        // 1. 체크박스 체크시 나머지 모든 항목 표시(Visible)
        //    => 단, 초기화와 종료버튼 제외하고 나머지 모든 항목
        checkStart.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(CompoundButton compoundButton, boolean isChecked) {
                // isChecked가 true일 경우 visibility 속성을 VISIBLE 로 변경
                // 아니면              ""                   INVISIBLE 또는 GONE으로 변경

//                if(isChecked) {
//                    tvPet.setVisibility(View.VISIBLE);
//                    rGroup.setVisibility(View.VISIBLE);
//                    btnOk.setVisibility(View.VISIBLE);
//                    ivPet.setVisibility(View.VISIBLE);
//                } else {
//                    tvPet.setVisibility(View.INVISIBLE);
//                    rGroup.setVisibility(View.INVISIBLE);
//                    btnOk.setVisibility(View.INVISIBLE);
//                    ivPet.setVisibility(View.INVISIBLE);
//                }

                int result = isChecked ? View.VISIBLE : View.INVISIBLE;
                tvPet.setVisibility(result);
                rGroup.setVisibility(result);
                btnOk.setVisibility(result);
                ivPet.setVisibility(result);

            }
        });

        // 2. 라디오버튼 선택 후 선택완료 버튼 클릭 시
        //    선택된 라디오버튼에 해당하는 이미지를 ImageView에 표시
        btnOk.setOnClickListener(view -> {

            switch (rGroup.getCheckedRadioButtonId()) {
                case R.id.rbDog:
                    ivPet.setImageResource(R.drawable.dog);
                    break;
                case R.id.rbCat:
                    ivPet.setImageResource(R.drawable.cat);
                    break;
                case R.id.rbRabbit:
                    ivPet.setImageResource(R.drawable.rabbit);
                    break;
                default:
                    Toast.makeText(MainActivity.this, "동물을 선택하세요!", Toast.LENGTH_SHORT).show();
            }
        });

        // 3. 초기화 버튼 클릭 시 숨김 처리 및 체크박스와 라디오버튼 선택 해제
        btnReset.setOnClickListener(view -> {
            // 숨김처리 (체크박스 초기화 시 자동으로 리스너를 통해 숨김처리가 동작
//            tvPet.setVisibility(View.INVISIBLE);
//            rGroup.setVisibility(View.INVISIBLE);
//            btnOk.setVisibility(View.INVISIBLE);
//            tvPet.setVisibility(View.INVISIBLE);

            // 라디오버튼 선택 초기화
            rGroup.clearCheck();

            // 체크박스 초기화
            checkStart.setChecked(false);

            // 이미지뷰 초기화
            ivPet.setImageResource(0);
        });

        // 4. 종료 버튼 클릭 시 프로그램 끝내기(종료)
        btnFinish.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                // 프로그램 종료하려면 finish() 메서드 호출
                finish();
            }
        });

    }
}
```

---

## 버튼 이동
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button"
        app:layout_constraintBottom_toTopOf="@+id/textView"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.498"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.459" />

    <Button
        android:id="@+id/button2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button"
        android:layout_marginStart="40dp"
        android:layout_marginTop="100dp"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/button3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="32dp"
        android:layout_marginTop="44dp"
        android:text="Button"
        app:layout_constraintStart_toEndOf="@+id/button2"
        app:layout_constraintTop_toTopOf="parent" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_begin="559dp" />

    <Button
        android:id="@+id/button4"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="40dp"
        android:text="Button"
        app:layout_constraintBottom_toTopOf="@+id/guideline2"
        app:layout_constraintStart_toStartOf="parent" />

    <Button
        android:id="@+id/button5"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginEnd="40dp"
        android:text="Button"
        app:layout_constraintBottom_toTopOf="@+id/guideline2"
        app:layout_constraintEnd_toEndOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

---

# [오후수업] JAVA 46차
> 테스트 실시.
