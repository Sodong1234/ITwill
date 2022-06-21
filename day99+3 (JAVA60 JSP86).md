# [오전수업] JAVA 60차
## Menu_ContextMenu
> - res 폴더에 New -> New Resource Directory에서 Resource type를 menu로 선택 후 생성
> - 생성한 menu 폴더에 menu1.xml, menu2.xml 생성

```xml
---------------------------------------------------activity_menu.xml---------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical"
    android:gravity="center"
    android:id="@+id/baseLayout">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="원하는 메뉴를 표시할 버튼을 롱클릭"
        android:textSize="20sp"
        android:gravity="center"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/btn1"
        android:text="배경색 변경"
        android:textSize="20sp"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/btn2"
        android:text="이미지 변경"
        android:textSize="20sp"/>

    <ImageView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/iv"
        android:layout_marginTop="100dp"
        android:src="@drawable/ic_launcher"/>


</LinearLayout>
---------------------------------------------------menu1.xml---------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">

    <item
        android:id="@+id/itemRed"
        android:title="배경색(빨강)"/>

    <item
        android:id="@+id/itemGreen"
        android:title="배경색(초록)"/>

    <item
        android:id="@+id/itemBlue"
        android:title="배경색(파랑)"/>
</menu>
---------------------------------------------------menu2.xml---------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">

    <item
        android:id="@+id/itemRotate"
        android:title="이미지 회전"/>

    <item
        android:id="@+id/itemExpand"
        android:title="이미지 확대"/>

</menu>
```
```java
package com.example.and0621_menu_contextmenu;

import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;

import android.graphics.Color;
import android.os.Bundle;
import android.view.ContextMenu;
import android.view.MenuInflater;
import android.view.MenuItem;
import android.view.View;
import android.widget.Button;
import android.widget.ImageView;
import android.widget.LinearLayout;

public class MainActivity extends AppCompatActivity {

    ImageView iv;
    LinearLayout baseLayout;
    Button btn1, btn2;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        iv = findViewById(R.id.iv);
        baseLayout = findViewById(R.id.baseLayout);
        btn1 = findViewById(R.id.btn1);
        btn2 = findViewById(R.id.btn2);

        // 컨텍스트 메뉴를 위젯에 등록하기 위해서는
        // registForContextMenu() 메서드를 호출하여 파라미터로 위젯 객체를 전달
        // => 아직 어떤 메뉴가 표시될지는 결정되지 않은 상태
        registerForContextMenu(btn1); // btn1 버튼위젯에 컨텍스트 메뉴 등록
        registerForContextMenu(btn2); // btn2 버튼위젯에 컨텍스트 메뉴 등록


        }

    @Override
        public void onCreateContextMenu(ContextMenu menu, View v, ContextMenu.ContextMenuInfo menuInfo) {
        super.onCreateContextMenu(menu, v, menuInfo);

        // 메뉴 등록을 위해서 MenuInflater 객체 필요
        MenuInflater menuInflater = getMenuInflater();

        // 2개 이상의 위젯에 각각 다른 메뉴를 등록해야할 경우
        // 파라미터로 전달된 View 객체 (v)를 각 위젯과 비교하여 롱클릭된 위젯 판별해야함
        if (v == btn1) { // v.getId() == R.id.btn1 비교도 결과 동일함
            // 배경색 변경 버튼에 menu1.xml 연결
            menuInflater.inflate(R.menu.menu1, menu);
            menu.setHeaderTitle("배경색 선택");

        } else if (v == btn2) { // v.getId() == R.id.btn2 비교도 결과 동일함

            menuInflater.inflate(R.menu.menu2, menu);
            menu.setHeaderTitle("이미지 작업 선택");
        }

    }
    // onContextItemSelected() 메서드를 오버라이딩 선택된 메뉴에 대한 작업 수행
    @Override
    public boolean onContextItemSelected(@NonNull MenuItem item) {
        super.onContextItemSelected(item);

        switch (item.getItemId()) {
            case R.id.itemRed:
                baseLayout.setBackgroundColor(Color.RED);
                break;
            case R.id.itemGreen:
                baseLayout.setBackgroundColor(Color.GREEN);
                break;
            case R.id.itemBlue:
                baseLayout.setBackgroundColor(Color.BLUE);
                break;
            case R.id.itemRotate:
                iv.setRotation(iv.getRotation() + 30);
                break;
            case R.id.itemExpand:
                iv.setScaleX(iv.getScaleX() + 2);
                iv.setScaleY(iv.getScaleY() + 2);
                break;


        }
        return true;


    }
}
```

## Activity_Basic
> - layout 폴더에서 New -> Layout Resource Directory 클릭 후 second_activity.xml 생성
> - java 폴더에서 New -> Java Class 클릭 후 SecondActivity 생성

```xml
---------------------------------------------------activity_main.xml---------------------------------------------------
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
        android:id="@+id/btnNewActivity"
        android:text="새 액티비티(화면) 열기"
        android:textSize="20sp"
        android:onClick="showNewActivity"/>

</LinearLayout>
---------------------------------------------------second_activity.xml---------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#FFFF88">

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/btnReturn"
        android:text="돌아가기"
        android:textSize="20sp"/>


</androidx.constraintlayout.widget.ConstraintLayout>
---------------------------------------------------AndroidManifest.xml---------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.and0621_activity_basic">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.And0621_Activity_Basic">
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <!-- 새로운 액티비티를 사용하기 위해서는 activity 태그로 액티비티 클래스 등록 필수! -->
        <activity android:name=".SecondActivity" android:label="두번째 액티비티"/>
    </application>

</manifest>
```
```java
---------------------------------------------------MainActivity.java---------------------------------------------------
package com.example.and0621_activity_basic;

import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.os.Bundle;
import android.view.View;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    public void showNewActivity(View v) {
        // 현재 액티비티에서 새로운 액티비티를 실행(전환)하기 위해서는
        // Intent 객체를 생성하여 전환활 액티비티 클래스를 지정하고
        // startActivity() 메서드를 호출하여 생성된 Intent 객체를 전달
        // => 주의! 전환할 액티비티 클래스 지정 시 "클래스명.class" 형태로 지정!
        Intent intent = new Intent(MainActivity.this, SecondActivity.class);
        // => 현재 객체 : MainActivity, 전환할 액티비티 : SecondActivity 클래스
        // => 주의! 새로운 액티비티로 전환하기 위해서는 해당 액티비티 클래스를
        //    반드시 AndroidManifests.xml 파일 내에 등록해야한다!
        //    ex) <activity android:name=".SecondActivity" android:label="두번째 액티비티"/>
        startActivity(intent);

    }

}
---------------------------------------------------second_activity.java---------------------------------------------------
package com.example.and0621_activity_basic;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;

import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;

// 액티비티 클래스 정의 시 반드시 Activity 클래스 상속 필수!
// => 최근 사용하는 클래스는 Activity 클래스의 서브클래스인
//    androidx.appcompat.app.AppCompatActivity
public class SecondActivity extends AppCompatActivity {
    // 액티비티 클래스 사용을 위해서는 해당 액티비티가 생성(메모리에 로딩) 될 때
    // 자동으로 호출되는 onCreate() 메서드 오버라이딩 필수!


    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        // 액티비티 클래스 하나당 화면을 구성하는 레이아웃(XML 파일) 하나를 연결
        // => 연결을 위해 setContentView() 메서드를 호출하여
        //    액티비티 실행 시 표시할 레이아웃(XML) 파일을 저장

        setContentView(R.layout.second_activity);

        Button btnReturn = findViewById(R.id.btnReturn);

        btnReturn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                // 현재 액티비티를 종료하고 이전 액티비티로 돌아가기 위해서는
                // finish() 메서드 호출
                finish();
            }
        });
    }
}

```

## Activity_Basic2
> Activity_Basic과 같은 방식으로 
---

# [오후수업] JSP 86차
```xml
---------------------------------------------------activity_main.xml---------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/btnNewActivity"
        android:text="새 액티비티(화면) 열기"
        android:textSize="20sp"/>

</LinearLayout>
---------------------------------------------------second_activity.xml---------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#FFFF88"
    android:orientation="vertical">

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/btnReturn"
        android:text="돌아가기"
        android:textSize="20sp"/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/btnThirdActivity"
        android:text="세번째 액티비티 열기"
       android:textSize="20sp"/>

</LinearLayout>
---------------------------------------------------third_activity.xml---------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#FF88FF">

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/btnReturn"
        android:text="돌아가기"
        android:textSize="20sp"/>



</LinearLayout>
---------------------------------------------------AndroidManifest.xml---------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.and0621_activity_basic2">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.And0621_Activity_Basic2">
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity android:name=".SecondActivity" android:label="두번째 액티비티"/>
        <activity android:name=".ThirdActivity" android:label="세번째 액티비티"/>
    </application>

</manifest>
```
```java
---------------------------------------------------MainActivity.java---------------------------------------------------
package com.example.and0621_activity_basic2;

import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button btnNewActivity = findViewById(R.id.btnNewActivity);
        btnNewActivity.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent intent =new Intent(MainActivity.this, SecondActivity.class);
                startActivity(intent);
            }
        });
    }
}
---------------------------------------------------SecondActivity.java---------------------------------------------------
package com.example.and0621_activity_basic2;

import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;

import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;

public class SecondActivity extends AppCompatActivity {
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.second_activity);

        Button btnReturn = findViewById(R.id.btnReturn);
        Button btnThirdActivity = findViewById(R.id.btnThirdActivity);

        btnReturn.setOnClickListener(view -> finish());

        btnThirdActivity.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent intent = new Intent(SecondActivity.this, ThirdActivity.class);
                startActivity(intent);
            }
        });
    }
}

---------------------------------------------------ThirdActivity.java---------------------------------------------------
package com.example.and0621_activity_basic2;

import android.os.Bundle;
import android.widget.Button;

import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;

public class ThirdActivity extends AppCompatActivity {
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.third_activity);

        Button btnReturn = findViewById(R.id.btnReturn);
        btnReturn.setOnClickListener(view -> finish());
    }
}

```
