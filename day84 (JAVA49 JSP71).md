# [오전수업] JAVA 49차
## TableLayout
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

  <!--
   TableLayout : 위젯을 테이블(표) 형태로 배치하는 레이아웃
   - layout_width, layout_height 속성을 사용하여 가로, 세로 크기를 지정
     => 별도로 행 또는 열의 크기는 설정 생략 가능함
        (width, height 속성 사용 시 해당 위젯 크기만 변경되는 것이 아니라
         열 또는 행 크기 중 열 크기는 연동되어 변경됨)
   - stretchColumns 속성을 사용하여 남은 공간을 할당받을 열 지정 가능
     => 복수개의 열을 콤마로 구분하여 지정하면 해당 열이 모두 공간 할당받음
   - collapseColumns 속성으로 숨길 열 지정
     => 단, stretchColumns 에 같은 열이 지정되어 있으면
        해당되는 열은 보이지는 않지만 영역은 차지함
   -->

    <TableLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:stretchColumns="1,2"
        android:collapseColumns="0">

        <TableRow>

            <Button
                android:text="버튼1"
                android:textSize="20sp"/>

            <Button
                android:text="버튼2"
                android:textSize="20sp"/>

            <Button
                android:text="버튼3"
                android:textSize="20sp"/>

        </TableRow>

        <TableRow>

            <Button
                android:text="버튼4"
                android:textSize="20sp"/>

            <Button
                android:text="버튼5"
                android:textSize="20sp"/>

            <Button
                android:layout_height="100dp"
                android:text="버튼6"
                android:textSize="20sp"/>

        </TableRow>

        <TableRow>
            <!-- 테이블 1개 행 내의 열은 각각의 위젯을 사용하여 생성 -->

            <Button
                android:text="버튼7"
                android:textSize="20sp"
                android:layout_span="2"/>
            <!-- layout_span 속성을 사용하면 열을 합칠 수 있다! -->

            <Button
                android:text="버튼8"
                android:textSize="20sp"/>

        </TableRow>

        <TableRow>

            <Button
                android:text="버튼9"
                android:textSize="20sp"
                android:layout_column="1"/>
            <!-- layout_column 속성 사용 시 표시할 컬럼 설정 가능 -->

            <Button
                android:text="버튼10"
                android:textSize="20sp"/>

        </TableRow>

    </TableLayout>


</LinearLayout>
```

- 연습) 계산기 만들기

```xml

```

---

# [오후수업] JSP 71차
