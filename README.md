# kioskDB

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
			primaryStage.setTitle("성일CAFE - 5월");
			primaryStage.show();
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
    
    @FXML
    public void M1pButtonAction(ActionEvent event) { // 키오스크 + 버튼을 누를 시 주문 갯수 증가
    	countm[0]=countm[0]+1;
    	menu_append();
    }

	private void menu_append() {		
		ListTextArea.setText("");
		for(int i=0; i<3;i++) {
			ListTextArea.appendText(menu_name[i] + " : " + countm[i] + "잔"+"\n");
		}	
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
    public void M1mButtonAction(ActionEvent event) {
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
    public void CancleButtonAction(ActionEvent event) {
    	sumLabel.setText("0");
    	ListTextArea.setText("");
    	for(int i=0;i<3; i++) {
    		countm[i]=0;
    	}	
    	sum = 0;
    }


    @FXML
    public void SumButtonAction(ActionEvent event) {
    	//메소드로 먼저 작성해보기 ==> 기본으로 ==> for문으로
    	//클래스로 별도 작성해보기	
    	sum = kiosksum.ksum(countm);
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
    
    }
    
    
    @FXML
    public void OrderButtonAction(ActionEvent event) {
    	//if(sum==0) { 경고메시지 }
    	//else if(sum!=0) {
    	//디비 접속
    	//실제 주문 내역 디비에 저장하기
    //}
    
    	if(sum!=0) {
    		DBconnect conn = new DBconnect();
    		Connection conn2 = conn.getconn();
    		
    		String sql = "insert into orderlist_accounts"
    				+ " (idx, order_time, menu1, count1, menu2, count2, menu3, count3, sum)"
    				+ " values (orderlist_idx_pk.nextval, current_timestamp, '아메리카노', ?, '카푸치노', ?, '카페라떼', ?, ?)";
    		
    		try {
				PreparedStatement ps = conn2.prepareStatement(sql);
				ps.setInt(1, countm[0]);
				ps.setInt(2, countm[1]);
				ps.setInt(3, countm[2]);
				ps.setInt(4, sum);
				
				ResultSet rs = ps.executeQuery();
				
				if(rs.next()) {
				
				sumLabel.setText("0");
		    	ListTextArea.setText("");
		    	for(int i=0;i<3; i++) {
		    		countm[i]=0;
		    }	
				sum = 0;
				
				Alert alert = new Alert(AlertType.INFORMATION);
				alert.setHeaderText("배달의 민족");
	    		alert.setContentText("배달의 민족 주문");
	    		alert.show();
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
