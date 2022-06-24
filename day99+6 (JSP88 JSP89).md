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
