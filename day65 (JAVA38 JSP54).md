# [오전수업] JAVA 38차

## Java Swing
- 자바에서 GUI 구현을 위해 제공되는 API
- 이전의 Java AWT(Active Window Toolkit)를 업그레이드하여 제공하는 패키지
  - AWT API와 Swing API를 조합하여 사용
- java.awt 패키지와 avax.swing 패키지의 각종 클래스 및 인터페이스 활용

---

*자주 사용되는 용어*
1. 컨테이너 : 여러 컴포넌트를 하나로 묶어서 부착 가능한 객체
- 프레임과 패널을 주로 사용
- JFrame 클래스와 JPanel 클래스로 구현
2. 컴포넌트 : 화면을 구성하는 각각의 요소
- 버튼, 체크박스, 라디오버튼, 텍스트필드, 텍스트에어리어
- 컨테이너에 부착하여 사용
- 각 컴포넌트에서 사용자로부터 어떤 동작이 발생하면 해당 동작을 처리하는 이벤트 처리 필요
>
- 창(윈도우)을 생성하기 위해서는 Window 계열 객체 생성 필요
  - 주로 JFrame 클래스를 사용
  - JFrame 객체를 생성하거나 JFrame 클래스를 상속받는 서브클래스를 정의하여 창 생성
    - ex) class Ex1 extends JFrame {} JFrame f = new JFrame();


### 화면을 표시하는 기능을 수행할 showFrame() 메서드
- JFrame 클래스의 각종 메서드를 사용하여 윈도우(창) 설정
1. setSize(가로픽셀, 세로픽셀) => 가로픽셀 * 세로픽셀 크기 지정
    - setLocation(가로좌표, 세로좌표) => 가로(x), 세로(y) 좌표 위치 지정
2. setTitle("제목") => 제목표시줄(타이틀)에 표시할 내용 지정
3. setDefaultCloseOperation() => 닫기 버튼 클릭 시 수행할 동작 지정
  - JFrame.XXX_ON_CLOSE 상수를 사용하여 닫기 버튼 클릭 시 수행할 동작 지정
    - EXIT_ON_CLOSE : 닫기 버튼 클릭 시 프로그램 종료 (따른 창도 모두 닫힘)
		- DISPOSE_ON_CLOSE : 닫기 버튼 클릭 시 현재 창만 종료 (닫힘)
		- HIDE_ON_CLOSE : 닫기 버튼 클릭 시 현재 창 숨김(종료X => 다시 표시 가능)
		- DO_NOTHING_ON_CLOSE : 닫기 버튼 클릭 시 아무 작업도 안함

4. setVisible(true or false) : 현재 프레임 표시 여부 결정(true : 표시, false : 숨김)

```java
package swing;

import javax.swing.JFrame;

public class Ex1 extends JFrame {
	
	public Ex1() {
		showFrame();
	}
	
	// 화면을 표시하는 기능을 수행할 showFrame() 메서드 정의
	public void showFrame() {
	
		setSize(300, 200);	// 가로 300픽셀, 세로 200픽셀 크기 설정
		setLocation(300, 350);	// 창이 생성될 좌표설정
		setTitle("JFrame을 상속받아 생성");	// 제목표시줄 타이틀 설정
		setDefaultCloseOperation(DO_NOTHING_ON_CLOSE);
	
		setVisible(true);
		
		// -----------------------------------------------------------------------
		// JFrame 객체를 직접 생성하여 프레임 생성
		JFrame f = new JFrame("JFrame 으로 생성한 프레임");
		f.setSize(600, 400);
		f.setLocation(600, 600);
		f.setTitle("타이틀 설정");
		f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		f.setVisible(true);
		
	}

	public static void main(String[] args) {
		new Ex1();
	}

}
```

연습문제
```java
package swing;

import javax.swing.JFrame;

/*
 * 400 * 400 크기의 창 생성
 * 제목 : JFrame 생성 연습
 * 닫기 버튼 클릭 시 프로그램 종료
 * 화면 표시 설정 : true
 * 
 * 
 */

public class Test1 extends JFrame {

	public Test1() {
		showFrame();
	}
	
	public void showFrame() {
		// FRame 객체 생성 version
//		JFrame f = new JFrame("JFrame 생성 연습");
//		f.setSize(400, 400);
//		f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
//		f.setVisible(true);
		
		// JFrame 상속 받는 version
		setTitle("JFrame 생성연습");
		setSize(400, 400);
		setDefaultCloseOperation(EXIT_ON_CLOSE);
		setVisible(true);
	}
	
	public static void main(String[] args) {

	}

}
```

```java
package swing;

import java.awt.Dimension;
import java.awt.Point;
import java.awt.Rectangle;

import javax.swing.JFrame;

public class Ex2 {

	public Ex2() {
		showFrame();
	}
	
	public void showFrame() {
		JFrame f = new JFrame();
//		f.setSize(600, 400);
		
		// 크기를 지정하는 setSize() 파라미터에 Dimension 객체 전달 가능하며
		// Dimension 객체 사용 시 크기를 지정하는 값을 관리할 수 있음
		// => 여러 컨테이너나 컴포넌트의 크기가 일정할 때 하나로 관리할 수 있음
//		Dimension d = new Dimension(600, 400);
//		f.setSize(d);
		
//		f.setLocation(800, 300);
		
		// 위치 좌표를 지정하는 setLocation() 파라미터에 Point 객체 전달 가능하며
		// Point 객체 사용 시 좌표를 지정하는 값을 관리 가능
		// => 여러 컨테이너나 컴포넌트의 위치가 일정할 때 하나로 관리할 수 있음
//		Point p = new Point(800, 300);
//		f.setLocation(p);
		
		// --------------------------------------------------------------
		// setBounds() 메서드를 사용하면 좌표와 크기를 동시에 설정 가능
		// 1) setBounds(x, y, width, height) : x좌표, y좌표, 가로크기, 세로크기 순으로 전달
//		f.setBounds(800, 300, 600, 400);
		
		// 2) setBounds(Rectangle r) : 좌표와 크기를 관리하는 Rectangle 객체 전달 가능
		// => Rectangle 객체 생성 시 파라미터로 x, y, width, height 전달 하거나 
		//	  Point p, Dimension d 객체 전달도 가능
//		Point p = new Point(800, 300);
//		Dimension d = new Dimension(600, 400);
//		Rectangle r = new Rectangle(p, d);
		
		Rectangle r = new Rectangle(800, 300, 600, 400);
		
		f.setBounds(r);
		
		f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		f.setVisible(true);
	}
	
	public static void main(String[] args) {
		new Ex2();
	}

}
```







---

# [오후수업] JSP 54차

