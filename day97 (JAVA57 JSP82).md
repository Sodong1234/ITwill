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
