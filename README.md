# kioskDB
<br><br><br>
# 키오스크 화면
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/698f154b-512e-4d3a-9885-026ff68e0c54)
<br><br><br>
# 관리자 로그인 화면
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/d11155f5-9b98-41d5-bd45-611edbf431ea)
<br><br><br>
# 통계 화면
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/ec2b8c22-d13a-45c3-a0ae-c4ae9d5847dd)
<br><br><br>
# kiosk main

```java
package application;
import javafx.application.Application;
import javafx.stage.Stage;
import javafx.scene.Parent;
import javafx.scene.Scene;
import javafx.fxml.FXMLLoader;

public class Main extends Application {
	@Override
	public void start(Stage primaryStage) {
		try {
			Parent root = FXMLLoader.load(getClass().getResource("Sample.fxml"));
			Scene scene = new Scene(root);
			scene.getStylesheets().add(getClass().getResource("application.css").toExternalForm());
			primaryStage.setScene(scene);
			primaryStage.setTitle("성일CAFE - 5월");	// 키오스크 화면 타이틀 이름
			primaryStage.show();	// 실행시 화면을 띄우는 코드(이게 있어야 화면이 나옴)
		} catch(Exception e) {
			e.printStackTrace();
		}
	}
	
	public static void main(String[] args) {
		launch(args);
	}
}

```
<br><br><br>
# controller code
``` java
public class SampleController {
    @FXML private Button CalButton, CancleButton;
    @FXML private Button M1pButton, M2pButton, M3pButton;
    @FXML private Button M1mButton, M2mButton, M3mButton;
    @FXML private TextArea ListTextArea;
    @FXML private Label sumLabel;
    @FXML private Button AdminButton, OrderButton;
    
    private int sum=0;
    private String[] menu_name = {"아메리카노", "카푸치노", "카페라떼"};  // 주문 할 품목 배열방으로 만들어놓기
    private int countm[] = new int[3];
    
    Kiosksum kiosksum = new Kiosksum();

	private void menu_append() {		
		ListTextArea.setText("");
		for(int i=0; i<3;i++) {
			ListTextArea.appendText(menu_name[i] + " : " + countm[i] + "잔"+"\n");
		}	
	}		// 메뉴 추가 버튼을 누를 시 아메리카노, 카푸치노, 카페라떼의 개수를 출력해줌
	@FXML
    public void M1pButtonAction(ActionEvent event) { 	// 키오스크 + 버튼을 누를 시 주문 개수 1증가
    	countm[0]=countm[0]+1;
    	menu_append();
    }
	@FXML
	public void M2pButtonAction(ActionEvent event) {
    	countm[1]++;    	
    	menu_append();
    }
    @FXML
    public void M3pButtonAction(ActionEvent event) {
    	countm[2]++;    	
    	menu_append();
    }
    @FXML
    public void M1mButtonAction(ActionEvent event) {	// -버튼을 누를 시 제품 개수 -1
    	if(countm[0]>0) countm[0]--;
    	menu_append();
    }
    @FXML
    public void M2mButtonAction(ActionEvent event) {
    	if(countm[1]>0) countm[1]--;
    	menu_append();
    }
    @FXML
    public  void M3mButtonAction(ActionEvent event) {
    	if(countm[2]>0) countm[2]--;
    	menu_append();
    }    
    @FXML
    public void CancleButtonAction(ActionEvent event) { // 취소버튼을 누를 시
    	sumLabel.setText("0");		// 총액 sumLabel text를 0으로 설정
    	ListTextArea.setText("");	// 제품을 모두 초기화
    	for(int i=0;i<3; i++) {
    		countm[i]=0;
    	}	
    	sum = 0;	// 합계 0으로 초기화
    }
    @FXML
    public void SumButtonAction(ActionEvent event) { // 합계 버튼을 누를 시
    	sum = kiosksum.ksum(countm);	// kiosksum 클래스에서 주문한 금액을 계산함
    	sumLabel.setText(sum + "");
    }
    @FXML
    public void AdminButtonAction(ActionEvent event) {
    	try {
			Parent root = FXMLLoader.load(getClass().getResource("adminlogin.fxml"));
			Scene scene = new Scene(root);
			Stage stage = new Stage();
			stage.setScene(scene);
			stage.setTitle("관리자 로그인 화면-5월");
			stage.show();
		} catch(Exception e) {
			e.printStackTrace();
		}
    }			// 관리자 로그인 버튼을 누를 시 adminlogin.fxml 에 만들어둔 화면을 띄우기
    @FXML
    public void OrderButtonAction(ActionEvent event) { // 주문버튼을 누를 시
    	//if(sum==0) { 경고메시지 }
    	//else if(sum!=0) {
    	//디비 접속
    	//실제 주문 내역 디비에 저장하기
    
    	if(sum!=0) {
    		DBconnect conn = new DBconnect();	// DB연결하는 코드
    		Connection conn2 = conn.getconn();
    		
    		String sql = "insert into orderlist_accounts" // 'orderlist_accounts' 테이블에 데이터를 삽입하는 INSERT문
    				+ " (idx, order_time, menu1, count1, menu2, count2, menu3, count3, sum)" 
    				+ " values (orderlist_idx_pk.nextval, current_timestamp, '아메리카노', ?, '카푸치노', ?, '카페라떼', ?, ?)";
    				// 삽입할 열은 idx, order_time, menu1, count1, menu2, count2, menu3, count3, sum
				// 값으로는 orderlist_idx_pk.nextval, current_timestamp, '아메리카노', ?, '카푸치노', ?, '카페라떼', ?, ?을 사용한다.
				// orderlist_idx_pk.nextval은 시퀀스 객체인 orderlist_idx_pk에서 다음 값을 가져오는 것을 나타낸다.
				// current_timestamp는 주문버튼을 눌렀을때 그 시간을 나타낸다.
				// ?에는 사용자가 메뉴를 주문한 주문 갯수가 들어간다.
    		try {
				PreparedStatement ps = conn2.prepareStatement(sql); // ?에 사용자가 입력한 값을 넣는 코드
				ps.setInt(1, countm[0]); // 첫번째(두, 세, 네번째도) ?에 countm[0](1, 2, sum)에 있는 값을 넣는다.
				ps.setInt(2, countm[1]);
				ps.setInt(3, countm[2]);
				ps.setInt(4, sum);
				ResultSet rs = ps.executeQuery(); // sql 쿼리 실행
				
				if(rs.next()) {
					sumLabel.setText("0");
		    			ListTextArea.setText("");
		    			for(int i=0;i<3; i++) {
		    				countm[i]=0;
					}	
					sum = 0;
				
					Alert alert = new Alert(AlertType.INFORMATION); // 정보창 띄우기
					alert.setHeaderText("배달의 민족");
	    				alert.setContentText("배달의 민족 주문");
	    				alert.show(); // stage를 보여주는 코드와 같은 역할
				} else {
					Alert alert = new Alert(AlertType.WARNING);
					alert.setHeaderText("배달의 민족");
		    			alert.setContentText("오류");
		    			alert.show();
				}
			} catch (SQLException e) {
				e.printStackTrace();
			}
    	} else {
    		Alert alert = new Alert(AlertType.WARNING);
    		alert.setContentText("메뉴를 선택하고 계산하기를 누르십시오");
    		alert.show();
    	}
    }
}
```
<br><br><br>
# 관리자 로그인 코드
```java
package application;
import java.io.IOException;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import javafx.event.ActionEvent;
import javafx.fxml.FXML;
import javafx.fxml.FXMLLoader;
import javafx.scene.Parent;
import javafx.scene.Scene;
import javafx.scene.control.Alert;
import javafx.scene.control.Alert.AlertType;
import javafx.scene.control.Button;
import javafx.scene.control.PasswordField;
import javafx.scene.control.TextField;
import javafx.stage.Stage;

public class AdminloginController {
	@FXML Button LoginButtion, ClearButton, CloseButton;
	@FXML TextField IdTextField;
	@FXML PasswordField PwPasswordField;
	@FXML
	private void LoginButtonAction(ActionEvent event) {
		// IdTextField에 입력한 Text가 비어있거나 PwPasswordField에도 비어있다면 경고창 띄우기
		if(IdTextField.getText().isEmpty() || PwPasswordField.getText().isEmpty()) {
			Alert alert = new Alert(AlertType.ERROR);
			alert.setTitle("경고창");
			alert.setContentText("아이디 비번 모두 입력");
			alert.show();
		} else {
		DBconnect conn = new DBconnect(); // DB연결 코드
		Connection conn2 = conn.getconn();
		
		String sql = "select adminid, adminpw"
				   + " from admin_accounts"
				   + " where adminid = ? and adminpw = ?";
		// DB에 있는 admin_accounts 테이블에 adminid, adminpw가 입력한 값과 일치한다면 adminid, pw 조회
		
		try {
			PreparedStatement ps = conn2.prepareStatement(sql);
			
			ps.setString(1, IdTextField.getText());		// 첫번째 ?값 가져오기
			ps.setString(2, PwPasswordField.getText());	// 두번째 ?값 가져오기
			
			ResultSet rs = ps.executeQuery(); // 쿼리 실행문
			if(rs.next()) {
				Alert alert = new Alert(AlertType.CONFIRMATION);
				alert.setTitle("알림");
				alert.setContentText("로그인 성공");
				alert.show();
				Stage stage = new Stage();
				
				CloseButtonAction(event);
				
				try {
					Parent root = FXMLLoader.load(getClass().getResource("Admindb.fxml"));
					Scene scene = new Scene(root);
					stage.setScene(scene);
					stage.show();
				} catch (IOException e) {
					
					e.printStackTrace();
				}
			} else {
				Alert alert = new Alert(AlertType.CONFIRMATION);
				alert.setTitle("알림");
				alert.setContentText("로그인 실패");
				alert.show();
			}
		} catch (SQLException e) {	
			e.printStackTrace();
		}
	}
}
	
	@FXML
	private void ClearButttonAction(ActionEvent event) {
		IdTextField.setText("");
		PwPasswordField.setText("");
	}
	@FXML
	private void CloseButtonAction(ActionEvent event) {
		Stage stage = new Stage();
		stage = (Stage)CloseButton.getScene().getWindow();
		stage.close();
	}
}
```
<br><br><br>
# DB연결 코드
```java
package application;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
public class DBconnect {
	
	public Connection conn; // conn이라는 DB와 연결해주는 코드
	public Connection getconn() {

		String driver = "oracle.jdbc.driver.OracleDriver"; // DB연결 경로(외워야함)
		String url = "jdbc:oracle:thin:@localhost:1521:xe";
		String id = "cafe3"; // DB생성 했을때 내가 지정한 id, pw
		String password = "cafe3";
		
		try {
			Class.forName(driver);
			conn = DriverManager.getConnection(url, id, password);	// DB에 있는 id, password가 일치 할 시 DB 접속
			System.out.println("디비 접속 성공-20230516");
		} catch (Exception e) {
			e.printStackTrace();
			System.out.println("디비 접속 실패");
		}
		return conn;
	}
}
```
<br><br><br>

# kiosksum 클래스
``` java
package application;
public class Kiosksum {
	public int ksum(int[] countm) {
		int sum2=0;
		int price[] = {1000,2000,3000};
			
		for(int i=0 ; i<3 ; i++) {
			sum2 = sum2 + countm[i] * price[i];
		}
		return sum2;
	}
}
```
