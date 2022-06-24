# JSP 88차, 89차

> 87차 수업 이어서 실시

```java
-------------------------------------------------------SimpleChatServer.java-------------------------------------------------------
package socket_programming;

import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.Font;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.EOFException;
import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;
import java.net.SocketException;

import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JTextArea;
import javax.swing.JTextField;

public class SimpleChatServer extends JFrame {
	private JLabel lblStatus;
	private JTextArea ta;
	private JTextField tf;

	private Socket socket;
	private ServerSocket serverSocket;

	private DataInputStream dis;
	private DataOutputStream dos;
	
	private boolean connectStatus; // 서버 접속 여부 저장
	private boolean stopSignal; // 멀티쓰레드 종료 여부 저장

	public SimpleChatServer() {
		showFrame();
	}

	public void showFrame() {
		setTitle("1:1 채팅 서버"); // 프레임 제목표시줄 텍스트 설정
		setBounds(400, 400, 500, 300); // x좌표, y좌표, 가로크기, 세로크기 설정
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); // 닫기버튼 기본 기능 설정(프로그램 종료)

		// 채팅창(프레임)의 상단(BorderLayout 의 NORTH 영역)에 상태표시창 표시
		lblStatus = new JLabel("클라이언트 연결 상태 : 없음");
		lblStatus.setFont(new Font("맑은 고딕", Font.ITALIC, 14));
		add(lblStatus, BorderLayout.NORTH);

		// 채팅 메세지를 표시할 JTextArea 생성 및 CENTER 영역에 표시
		ta = new JTextArea();
		ta.setFont(new Font("맑은 고딕", Font.BOLD, 16));
		ta.setEditable(false); // 텍스트 입력 금지
		ta.setBackground(Color.LIGHT_GRAY);
		add(ta, BorderLayout.CENTER);

		// 채팅 메세지를 입력할 JTextField 생성 및 SOUTH 영역에 표시
		tf = new JTextField();
		tf.setFont(new Font("맑은 고딕", Font.PLAIN, 15));
		add(tf, BorderLayout.SOUTH);
		// 텍스트필드에서 엔터키 입력 시 동작하는 이벤트 처리 - ActionListener 인터페이스 활용
		tf.addActionListener(new ActionListener() {

			@Override
			public void actionPerformed(ActionEvent e) {
				// 입력받은 메세지 전송을 수행할 sendMessage() 메서드 호출
				sendMessage();

			}
		});

		setVisible(true); // 프레임 화면에 표시

		tf.requestFocus(); // 텍스트필드에 커서 요청

		startService();
	}

	// 채팅 서비스를 수행하는 startService() 메서드 정의
	public void startService() {
		try {
			ta.append("채팅 서비스 준비중...\n");

			// ServerSocket 객체를 생성하여 지정된 포트(60000)를 개방(바인딩 = Binding)
			int port = 60000;
			serverSocket = new ServerSocket(port);
			ta.append("서비스 준비 완료!\n");

			ta.append("클라이언트 접속 대기중...\n");
			// ServerSocket 객체의 accept() 메서드를 호출하여 클라이언트 연결 대기
			// => 연결 성공 시 Socket 객체 반환되므로 저장
			socket = serverSocket.accept();
			ta.append("클라이언트가 접속하였습니다(" + socket.getInetAddress() + ")\n");

			lblStatus = new JLabel("클라이언트 연결 상태 : 연결됨(" + socket.getInetAddress() + ")");
			
			dis = new DataInputStream(socket.getInputStream());
			dos = new DataOutputStream(socket.getOutputStream());

			// 만약, receiveMEssage() 메서드 호출 이후 다른 동작이 있을 경우
			// receiveMessage() 메서드 내에서 while(true) 문의 무한루프로 인해
			// 다른 동작이 수행되지 못할 수 있다!
			// 따라서, 이러한 작업은 다른 작업과 별개로 동작할 수 있도록 멀티쓰레드로 처리해야함
			receiveMessage();

			// Thread 객체 생성 시 파라미터로 Runnable 인터페이스를 구현하여 멀티쓰레드 구현 가능
//			new Thread(new Runnable() {
//				
//				@Override
//				public void run() {
//					receiveMessage();
//				}
//			}).start();

		} catch (EOFException | SocketException e) {
			ta.append("클라이언트로부터 접속이 해제되었습니다!\n");
			try {
				dis.close();
				dos.close();
				socket.close();
				
				stopSignal = true;
			} catch (IOException e1) {
				e1.printStackTrace();
			}
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

	public void receiveMessage() {
		new Thread(new Runnable() {

			@Override
			public void run() {
				// 클라이언트로부터 수신되는 메세지 입력받아 출력 => 무한 루프 사용하여 반복
				try {
					while (!stopSignal) {
						String msg = dis.readUTF();
						ta.append("클라이언트 : " + msg + "\n");
					}
				} catch (EOFException | SocketException e) {
					ta.append("클라이언트로부터 접속이 해제되었습니다!\n");
					try {
						dis.close();
						dos.close();
						socket.close();
						
						stopSignal = true;
					} catch (IOException e1) {
						e1.printStackTrace();
					}
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		}).start();
	}

	// 입력받은 메세지 전송을 수행하는 sendMessage() 메서드 정의
	public void sendMessage() {
		try {
			// 입력받은 메세지(JTextField 객체의 입력값)를 가져오기
			String msg = tf.getText();
//		System.out.println("서버 : " + msg);
			// 입력받은 메세지를 텍스트에어리어에 출력
			ta.append(msg + "\n");

			// 입력된 메세지를 클라이언트에게 전송
			dos.writeUTF(msg);

			// 텍스트필드 입력값 초기화 및 커서 요청
			tf.setText("");
			tf.requestFocus();
		} catch (IOException e) {
			e.printStackTrace();
		}

	}

	public static void main(String[] args) {
		new SimpleChatServer();
	}

}

-------------------------------------------------------SimpleChatClient.java-------------------------------------------------------
package socket_programming;

import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.FlowLayout;
import java.awt.Font;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.EOFException;
import java.io.IOException;
import java.net.Socket;
import java.net.SocketException;
import java.net.UnknownHostException;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JTextArea;
import javax.swing.JTextField;

public class SimpleChatClient extends JFrame {
	private JLabel lblStatus;
	private JTextArea ta;
	private JTextField tf;
	private JButton btnConnect;

	private Socket socket;

	private DataInputStream dis;
	private DataOutputStream dos;

	private boolean connectStatus; // 서버 접속 여부 저장
	private boolean stopSignal; // 멀티쓰레드 종료 여부 저장

	public SimpleChatClient() {
		showFrame();
	}

	public void showFrame() {
		setTitle("1:1 채팅 클라이언트"); // 프레임 제목표시줄 텍스트 설정
		setBounds(400, 400, 500, 300); // x좌표, y좌표, 가로크기, 세로크기 설정
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); // 닫기버튼 기본 기능 설정(프로그램 종료)

		// 채팅창(프레임)의 상단(BorderLayout 의 NORTH 영역)에 상태표시창 및 버튼 표시를 위한 패널
		JPanel pNorth = new JPanel(new FlowLayout(FlowLayout.RIGHT));
		// 생성된 패널을 NORTH 영역에 부착
		add(pNorth, BorderLayout.NORTH);

		// 상태표시창을 JLabel 객체로 생성 후 패널에 부착
		lblStatus = new JLabel("클라이언트 연결 상태 : 없음");
		lblStatus.setFont(new Font("맑은 고딕", Font.ITALIC, 14));
		pNorth.add(lblStatus);

		// 서버연결 버튼 생성 후 패널에 부착
		btnConnect = new JButton("서버연결");
		pNorth.add(btnConnect);
		// 서버연결에 대한 이벤트 처리
		btnConnect.addActionListener(new ActionListener() {

			@Override
			public void actionPerformed(ActionEvent e) {
				// 서버 접속 여부에 따라 다른 메서드 호출
				if (connectStatus) { // 접속되어 있을 경우
					disconnectServer();
				} else { // 접속되어 있지 않을 경우
					startService();
				}
			}
		});

		// 채팅 메세지를 표시할 JTextArea 생성 및 CENTER 영역에 표시
		ta = new JTextArea();
		ta.setFont(new Font("맑은 고딕", Font.BOLD, 16));
		ta.setEditable(false); // 텍스트 입력 금지
		ta.setBackground(Color.LIGHT_GRAY);
		add(ta, BorderLayout.CENTER);

		// 채팅 메세지를 입력할 JTextField 생성 및 SOUTH 영역에 표시
		tf = new JTextField();
		tf.setFont(new Font("맑은 고딕", Font.PLAIN, 15));
		add(tf, BorderLayout.SOUTH);

		// 텍스트필드에서 엔터키 입력 시 동작하는 이벤트 처리 - ActionListener 인터페이스 활용
		tf.addActionListener(new ActionListener() {

			@Override
			public void actionPerformed(ActionEvent e) {
				// 입력받은 메세지 전송을 수행할 sendMessage() 메서드 호출
				sendMessage();

			}
		});

		setVisible(true); // 프레임 화면에 표시

		tf.requestFocus(); // 텍스트필드에 커서 요청

	}

	public void startService() {
		try {
			ta.append("채팅 서버에 접속을 시도중입니다...\n");
			// Socket 객체를 생성 시 IP 주소와 포트번호 전달하여 접속 시도
			String host = "localhost";
			int port = 60000;
			socket = new Socket(host, port);
			ta.append("채팅 서버 접속 완료!\n");

			lblStatus.setText("클라이언트 연결 상태 : 연결됨");
			btnConnect.setText("채팅종료");
			connectStatus = true;

			dis = new DataInputStream(socket.getInputStream());
			dos = new DataOutputStream(socket.getOutputStream());

			receiveMessage();
		} catch (EOFException | SocketException e) {
			ta.append("서버로부터 접속이 해제되었습니다!\n");
			try {
				dis.close();
				dos.close();
				socket.close();
			} catch (IOException e1) {
				e1.printStackTrace();
			}
		} catch (UnknownHostException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

	public void receiveMessage() {
		new Thread(new Runnable() {

			@Override
			public void run() {
				try {
					while (!stopSignal) { // 중지 신호가 false 일 동안 반복
						// 서버로부터 수신되는 메세지 입력받아 출력 => 무한 루프 사용하여 반복
						// => 단, 외부로부터 중지 신호가 들어오면(= true) 반복 중지
						String msg = dis.readUTF();
						ta.append("서버 : " + msg + "\n");
					}
				} catch (EOFException | SocketException e) {
					ta.append("서버로부터 접속이 해제되었습니다!\n");
					disconnectServer();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		}).start();
	}

	// 입력받은 메세지 전송을 수행하는 sendMessage() 메서드 정의
	public void sendMessage() {
		try {
			// 입력받은 메세지(JTextField 객체의 입력값)를 가져오기
			String msg = tf.getText();
//			System.out.println("서버 : " + msg);
			// 입력받은 메세지를 텍스트에어리어에 출력
			ta.append(msg + "\n");

			// 입력된 메세지를 클라이언트에게 전송
			dos.writeUTF(msg);

			// 텍스트필드 입력값 초기화 및 커서 요청
			tf.setText("");
			tf.requestFocus();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

	public void disconnectServer() {
		try {
			// 모든 자원 반환
			dis.close();
			dos.close();
			socket.close();

			lblStatus.setText("서버 연결 상태 : 없음");
			btnConnect.setText("채팅시작");
			connectStatus = false;
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

	public static void main(String[] args) {
		new SimpleChatClient();
	}

}

```

---

> Android Studio로 수업 실시

## Socket_Programming_Basic

```xml
-----------------------------------------------------activity_main.xml-----------------------------------------------------
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
        android:background="#CCCCFF"
        android:orientation="vertical">

       <LinearLayout
           android:layout_width="match_parent"
           android:layout_height="wrap_content">

        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:singleLine="true"
            android:id="@+id/etMsg"
            android:layout_weight="1"/>

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/btnSend"
            android:text="전송"/>


    </LinearLayout>


            <TextView
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:id="@+id/tvMsg"/>

        </LinearLayout>



<LinearLayout
android:layout_width="match_parent"
android:layout_height="0dp"
android:layout_weight="1"
android:background="#CCFFCC"
    android:orientation="vertical">

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/btnStart"
        android:text="서버시작"/>


            <TextView
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:id="@+id/tvMsg2"/>


    </LinearLayout>

</LinearLayout>
-----------------------------------------------------AndroidManifest.xml-----------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.and0624_socket_programming_basic">

    <!-- 통신(인터넷) 권한을 얻기 위해 uses-permission 태그를 사용하여 INTERNET 권한 부여 -->
    <uses-permission android:name="android.permission.INTERNET"/>
    
    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="소켓프로그래밍"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.And0624_Socket_Programming_Basic">
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```
```java
package com.example.and0624_socket_programming_basic;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.os.Handler;
import android.text.method.ScrollingMovementMethod;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;

import com.example.and0624_socket_programming_basic.R;

import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;

public class MainActivity extends AppCompatActivity {

    private EditText etMsg;
    private TextView tvMsg, tvMsg2;

    Handler handler = new Handler();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        etMsg = findViewById(R.id.etMsg);
        tvMsg = findViewById(R.id.tvMsg);
        tvMsg2 = findViewById(R.id.tvMsg2);
        tvMsg.setMovementMethod(new ScrollingMovementMethod());
        tvMsg2.setMovementMethod(new ScrollingMovementMethod());


        Button btnSend = findViewById(R.id.btnSend);
        Button btnStart = findViewById(R.id.btnStart);

        btnSend.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                // EditText 에 입력된 텍스트 가져오기
                String msg = etMsg.getText().toString();
                Toast.makeText(MainActivity.this, msg, Toast.LENGTH_SHORT).show();

                // 클라이언트에서 입력한 메세지를 서버측으로 전송하기 위한 sendMsg() 메서드 호출
                // => 단, 이 작업을 멀티 쓰레드 환경으로 구현하기 위해 Thread 클래스 활용

                new Thread(new Runnable() {
                    @Override
                    public void run() {
                        sendMsg(msg);
                    }
                }).start();

                etMsg.setText("");
                etMsg.requestFocus();
            }
        });


        btnStart.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                // 서버를 시작하기 위한 startServer() 메서드 호출 => 단, 멀티쓰레딩으로 구현

                new Thread(new Runnable() {
                    @Override
                    public void run() {
                        startServer();
                    }
                }).start();
            }
        });
    }

    public void printClient(String msg) {  // Client 메세지 출력
        // Handler 객체의 post() 메서드 호출 => 쓰레드 환경과 유사하므로 Runnable 인터페이스 구현
        handler.post(new Runnable() {
            @Override
            public void run() {
                // 입력받은 메세지를 TextView 에 출력 (기존 메세지 뒤에 추가)
                tvMsg.append(msg + "\n");
            }
        });
    }

    public void printServer(String msg) {  // Server 메세지 출력
        handler.post(new Runnable() {
            @Override
            public void run() {
                tvMsg2.append(msg + "\n");
            }
        });
    }

    public void sendMsg(String msg) {    // Client -> Server로 메세지 출력
        try {
            // 서버 접속
            String host = "localhost";
            int port = 60000;

            Socket socket = new Socket(host, port);
            printClient("서버 연결됨");

            // 문자열 수신을 위한 DataInputStream 객체 생성
            DataInputStream dis = new DataInputStream(socket.getInputStream());

            // 문자열 송신을 위한 DataOutputStream 객체 생성
            DataOutputStream dos = new DataOutputStream(socket.getOutputStream());

            // 주의! 메세지 송신과 수신이 하나의 메서드 내에서 수행되므로
            // 클라이언트 -> 서버로 메세지 송신을 먼저 수행한 후
            // 서버 -> 클라이언트로 메세지 수신을 수행해야한다! (순서 중요!)
            // 단, 두작업을 별개의 쓰레드로 구현 시 순서 무관함
            // 서버에게 입력받은 메세지 전송
            dos.writeUTF(msg);
            dos.flush(); // 출력 스트림의 남은 모든 데이터 내보내기(스트림 비우기)

            // 서버로부터 전송되는 메세지 (문자열) 읽어와서 TextView 에 출력
            printClient("클라이언트 : " + dis.readUTF() + "\n");


            // 클라이언트측 TextView 에 송신완료 메시지 출력
            printClient("서버로 메세지 전송 완료!");

            // 소켓반환 (주의! ServerSocket 객체는 계속 대기를 위해 반환하지 않음)
            socket.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public void startServer() {  // Server를 시작
        try {
            int port = 60000;   // 서버에서 바인딩 할 포트번호
            ServerSocket serverSocket = new ServerSocket(port); // 서버소켓 생성
            printServer("서버시작");

            while (true) {    // 무한 루프를 사용하여 클라이언트 접속 처리 및 메세지 수신 작업 수행
                // 클라이언트 접속 대기
                Socket socket = serverSocket.accept();
                printServer("클라이언트 연결됨 -" + socket.getInetAddress() + ":" + socket.getPort());

                // 문자열 수신을 위한 DataInputStream 객체 생성
                DataInputStream dis = new DataInputStream(socket.getInputStream());

                // 문자열 송신을 위한 DataOutputStream 객체 생성
                DataOutputStream dos = new DataOutputStream(socket.getOutputStream());

                // 주의! 클라이언트가 접속 후 바로 메세지를 전송하므로
                // 먼저 클라이언트 -> 서버로 전송되는 메세지를 수신한 후
                // 서버 -> 클라이언트로 전송되는 메세지를 송신해야한다! (순서중요)
                // 단, 두 작업을 별개의 쓰레드로 수행할 경우 순서 무관
                // 클라이언트로부터 전송되는 메세지 (문자열) 읽어와서 TextView 에 출력
                printServer("클라이언트 : " + dis.readUTF() + "\n");

                // 클라이언트에게 "메세지 수신 확인 - From server" 메세지 전송
                dos.writeUTF("메세지 수신확인 - FROM server");
                dos.flush();    // 출력 스트림의 남은 모든 데이터 내보내기(스트림 비우기)

                // 서버측 TextView 에 송신완료 메시지 출력
                printServer("클라이언트로 메세지 전송 완료!");

                // 소켓반환 (주의! ServerSocket 객체는 계속 대기를 위해 반환하지 않음)
                socket.close();
            }


        } catch (IOException e) {
            e.printStackTrace();
        }
    }


}
```

## Http_Request_with_Volley

> - Dependency 추가하기
> - File -> Project Struture -> Dependency -> app 폴더 클릭 -> + 버튼 클릭 -> Library Dependency -> com.android.volley* 검색 -> Artfact NAme volley * Versions 1.2.1 선택
>   - Gradle Scripts -> bulid gradle에서 정상적으로 생성되었는지 확인 가능

```xml
-----------------------------------------------------activity_mail.xml-----------------------------------------------------
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
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/etUrl"
        android:singleLine="true"
        android:hint="요청 URL 입력"/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/btnRequest"
        android:text="요청하기"
        android:textSize="20sp"/>

    <TextView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/tvResult"
        android:background="#88AAAAAA"/>


</LinearLayout>
-----------------------------------------------------AndroidManifest.xml-----------------------------------------------------
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.and0624_http_request_with_volley">

    <uses-permission android:name="android.permission.INTERNET"/>


    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.And0624_Http_Request_with_Volley">
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```
```java
package com.example.and0624_http_request_with_volley;

import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.text.method.ScrollingMovementMethod;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;

import com.android.volley.AuthFailureError;
import com.android.volley.Request;
import com.android.volley.RequestQueue;
import com.android.volley.Response;
import com.android.volley.VolleyError;
import com.android.volley.toolbox.StringRequest;
import com.android.volley.toolbox.Volley;

import java.util.HashMap;
import java.util.Map;

public class MainActivity extends AppCompatActivity {
    EditText etUrl;
    Button btnRequest;
    TextView tvResult;

    // 요청(Reqeust) 객체를 관리하는 RequestQueue 타입 변수 선언
    // => 한 번 생성 후 재사용하기 위해 static 변수로 선언
    static RequestQueue requestQueue;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);



        etUrl = findViewById(R.id.etUrl);
        btnRequest = findViewById(R.id.btnRequest);
        tvResult = findViewById(R.id.tvResult);
        tvResult.setMovementMethod(new ScrollingMovementMethod()); // TextView 스크롤 기능 추가

        // 임시 URL 정보 변수에 저장 및 EditText 에 입력
        String url = "https://kobis.or.kr/kobisopenapi/webservice/rest/boxoffice/searchDailyBoxOfficeList.json?key=f5eef3421c602c6cb7ea224104795888&targetDt=20220503";
        etUrl.setText(url);

        btnRequest.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Toast.makeText(MainActivity.this, "클릭", Toast.LENGTH_SHORT).show();
                makeRequest();
            }
        });

        // Volley API 에서 요청 정보를 관리하는 ReqeustQueue 객체가 비어있을 경우 객체 생성
        if(requestQueue == null) {
            // Volley 클래스의 newRequestQueue() 메서드를 호출하여 객체 생성
            // => 이 때, 파라미터로 현재 액티비티(컨텍스트) 전달
            requestQueue = Volley.newRequestQueue(this);
        }
    }


    // 요청 정보를 생성하여 요청을 수행할 makeRequest() 메서드 정의
    public void makeRequest() {
        // 입력받은 URL 가져오기
        String strUrl = etUrl.getText().toString();
//        Toast.makeText(this, strUrl, Toast.LENGTH_SHORT).show();
        
        // StringRequest 객체 생성
        // 파라미터
        // 1) 요청 방식(Request.Method.XXX 상수 사용)
        // 2) 요청 URL
        // 3) 응답을 처리할 리스너 구현(Response.Listener 구현체 활용)
        // 4) 에러를 처리할 리스너 구현(Response.ErrorListener 구현체 활용)
        StringRequest request = new StringRequest(
                Request.Method.GET, // 요청 방식
                strUrl, // 요청 URL
                new Response.Listener<String>() { // 응답 리스너
                    @Override
                    public void onResponse(String response) { // 응답 메세지 수신 시 자동 호출됨
                        // 응답 메세지를 TextView 에 출력
                        showMessage("[ 응답 데이터 ]\n" + response);
                    }
                },
                new Response.ErrorListener() { // 에러 리스너
                    @Override
                    public void onErrorResponse(VolleyError error) { // 에러 메세지 수신 시 자동 호출됨
                        // 에러 메세지를 TextView 에 출력
                        // => 자바 Exception(예외) 처리 시 catch 블럭과 사용법 유사함
                        showMessage("[ 에러 메세지 ]\n" + error.getMessage());
                    }
                }

        ){ // POST 방식 요청일 경우 구현할 부분(StringRequest 생성자 뒤에 중괄호() 를 기술하여 구현)
            // 단, 이 중괄호 블럭은 POST 방식이 아닐 경우 불필요하므로 생략 가능
            // getParams() 메서드 오버라이딩
            @Nullable
            @Override
            protected Map<String, String> getParams() throws AuthFailureError {
                // POST 방식에서 전송할 파라미터를 Map<String, String> 객체로 생성 후 리턴
                Map<String, String> params = new HashMap<String, String>();
                // 파라미터가 있을 경우 params.put(key, Value) 형식으로 저장
                return params; // 파라미터 객체(Map) 리턴

            }
        }; // StringRequest 객체 생성 끝

        // 요청 객체(StringRequest) 생성 완료 후 요청 큐(RequestQueue)에 추가
        // => RequestQueue 객체의 add() 메서드를 호출하여 생성된 요청 객체 전달
        // => 만약, 이전 요청으로부터 응답받은 응답 결과를 재사용하지 않을 경우
        //    요청 객체의 setShouldCache() 메서드를 호출하여 false 값으로 설정
        request.setShouldCache(false);
        requestQueue.add(request);
        showMessage("요청 완료!");

    }

    // TextView 에 메세지를 출력하는 showMessage() 메서드 정의 출력
    private void showMessage(String str) {
        tvResult.append(str + "\n");
    }
}
```

## Http_Request_with_Volley_with_Gson

> Movie & MovieListBoxOfficeResult & MovieList 클래스 생성

```xml
```
```java
-----------------------------------------------------MainActivity.java-----------------------------------------------------
-----------------------------------------------------Movie.java-----------------------------------------------------
package com.example.and0624_http_request_with_volley_with_gson;

public class Movie {
    /*
    JSON 객체의 각 파라미터에 해당하는 멤버변수를 선언하고
    파싱된 각 영화의 상세 정보를 저장하는 용도의 클래스 정의 = DTO(JavaBean, VO) 클래스 역할
    => 1개 영화 정보를 저장하는 객체 역할 수행
    < 주의! > JSON 객체가 갖는 파라미터 이름과 멤버변수명이 동일해야함
    다음 URL 에서 응답 구조 부분을 참조하여 변수 선언
    http://www.kobis.or.kr/kobisopenapi/homepg/apiservice/searchServiceInfo.do?serviceId=searchDailyBoxOffice
     */

    private String rNum;
    private String rank;
    private String rankInten;
    private String rankOldAndNew;
    private String movieCd;
    private String movieNm;
    private String openDt;
    private String salesAmt;
    private String salesShare;
    private String salesInten;
    private String salesChange;
    private String salesAcc;
    private String audiCnt;
    private String audiInten;
    private String audiChange;
    private String audiAcc;
    private String scrnCnt;
    private String showCnt;

    public String getrNum() {
        return rNum;
    }

    public void setrNum(String rNum) {
        this.rNum = rNum;
    }

    public String getRank() {
        return rank;
    }

    public void setRank(String rank) {
        this.rank = rank;
    }

    public String getRankInten() {
        return rankInten;
    }

    public void setRankInten(String rankInten) {
        this.rankInten = rankInten;
    }

    public String getRankOldAndNew() {
        return rankOldAndNew;
    }

    public void setRankOldAndNew(String rankOldAndNew) {
        this.rankOldAndNew = rankOldAndNew;
    }

    public String getMovieCd() {
        return movieCd;
    }

    public void setMovieCd(String movieCd) {
        this.movieCd = movieCd;
    }

    public String getMovieNm() {
        return movieNm;
    }

    public void setMovieNm(String movieNm) {
        this.movieNm = movieNm;
    }

    public String getOpenDt() {
        return openDt;
    }

    public void setOpenDt(String openDt) {
        this.openDt = openDt;
    }

    public String getSalesAmt() {
        return salesAmt;
    }

    public void setSalesAmt(String salesAmt) {
        this.salesAmt = salesAmt;
    }

    public String getSalesShare() {
        return salesShare;
    }

    public void setSalesShare(String salesShare) {
        this.salesShare = salesShare;
    }

    public String getSalesInten() {
        return salesInten;
    }

    public void setSalesInten(String salesInten) {
        this.salesInten = salesInten;
    }

    public String getSalesChange() {
        return salesChange;
    }

    public void setSalesChange(String salesChange) {
        this.salesChange = salesChange;
    }

    public String getSalesAcc() {
        return salesAcc;
    }

    public void setSalesAcc(String salesAcc) {
        this.salesAcc = salesAcc;
    }

    public String getAudiCnt() {
        return audiCnt;
    }

    public void setAudiCnt(String audiCnt) {
        this.audiCnt = audiCnt;
    }

    public String getAudiInten() {
        return audiInten;
    }

    public void setAudiInten(String audiInten) {
        this.audiInten = audiInten;
    }

    public String getAudiChange() {
        return audiChange;
    }

    public void setAudiChange(String audiChange) {
        this.audiChange = audiChange;
    }

    public String getAudiAcc() {
        return audiAcc;
    }

    public void setAudiAcc(String audiAcc) {
        this.audiAcc = audiAcc;
    }

    public String getScrnCnt() {
        return scrnCnt;
    }

    public void setScrnCnt(String scrnCnt) {
        this.scrnCnt = scrnCnt;
    }

    public String getShowCnt() {
        return showCnt;
    }

    public void setShowCnt(String showCnt) {
        this.showCnt = showCnt;
    }
}


-----------------------------------------------------MovieListBoxOfficeResult.java-----------------------------------------------------
package com.example.and0624_http_request_with_volley_with_gson;

import java.util.ArrayList;

public class MovieListBoxOfficeResult {
    // 박스오피스 검색 결과 전체를 저장할 클래스
    
    private String boxofficeType; // 박스오피스 종류(소문자 o 주의!)
    private String showRange; // 검색 범위
    
    // 검색 결과인 영화 정보의 목록을 저장하기 위해
    // Movie 타입 객체를 복수개 저장하기 위한 ArrayList 타입 변수 선언(제네릭타입 Movie)
    private ArrayList<Movie> dailyBoxOfficeList;
}

-----------------------------------------------------MovieList.java-----------------------------------------------------
```

> 다음 수업시간에 이어서 할 예정
