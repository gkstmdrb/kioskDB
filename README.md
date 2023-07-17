# kioskDB
<br><br><br>
# 키오스크 화면
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/698f154b-512e-4d3a-9885-026ff68e0c54) <br>
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/aeaa58c0-5199-49bd-9c45-33cd14945394) <br><br>
SceneBuilder에서 디자인한 화면을 띄우는 코드이다.

## 품목 버튼
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/1a0fc4cf-9396-4885-b00c-a57a4d45ab09) <br>
+버튼을 누르면 원하는 품목의 개수가 1개 늘어나고, <br>
-버튼을 누르면 개수가 1개 차감된다. <br>
계산하기 버튼을 누르면 주문한 품목의 총액을 계산하여 금액이 나온다.

## 주문시
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/861e9335-33dd-4bf7-b47a-b96b05677b70) <br>
계산시 총액이 0원이 아니라면 주문 성공시 나오는 안내 화면이 나오고, <br>
DB에 내가 주문한 정보가 저장된다. <br><br>
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/508a4474-8c34-440b-85d8-d1dc97819b64) <br>
합계가 0원이라면 DB에 정보가 저장이 되지도 않고, <br>
다시 주문해야 한다.

<br><br><br>
# 관리자 로그인 화면
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/d11155f5-9b98-41d5-bd45-611edbf431ea) <br>
## 로그인 화면 관련 코드
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/0458a498-35e4-4c70-a285-88375520e9ef) <br>
입력하는 IdTextField와 PwPasswordField가 하나라도 비어있다면, <br>
경고창이 나오고 다시 입력해야한다. <br>
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/73368a3d-345e-476b-b398-6ec7d3836ef9) <br><br>

## 비어있지 않다면
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/438e598b-d018-44b9-86ff-5051edbcab33) <br><br>
DB에 연결하여 select문을 통해 admin_accounts 테이블에 있는 정보와 비교하여 <br>
일치하다면 로그인 성공 안내창이 나오고 다르다면 로그인 실패 안내창이 나온다. <br><br>
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/299aaa0c-4e7b-4ca1-8300-06f329d92d01) <br>
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/0493f829-55ae-4929-9b6f-2bca9b80d4ae)

<br><br><br>
# 통계 화면
## 전체조회 후 판매수량 그래프
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/ec2b8c22-d13a-45c3-a0ae-c4ae9d5847dd) <br>
## 전체조회 버튼 코드
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/1e81bb51-0739-40f1-a94c-45d75203ed75) <br>
전체조회 버튼을 누르면 DB접속 후 select문으로 orderlist_accounts 테이블에 있는 <br>
주문번호, 주문시간, 주문개수, 합계를 주문번호 순으로 정렬하여 <br>
orderlistTableView인 <br>
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/b3201ba1-172a-4626-87f3-d26cdbde1084) <br>
화면에는 주문번호, 일시, 아메리카노, 카푸치노, 카페라떼의 개수, 합계가 나오고 <br><br>

resultTextArea인 <br>
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/20007359-2e35-4da8-8839-70e6e10341b5) <br>
화면에는 아메리카노, 카푸치노, 카페라떼의 개수와, 금액의 총합이 나온다. <br><br>

![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/9f003c16-b7b5-4aa8-8d34-9bd7af7b59b7) <br>
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/ae36a673-f1e1-4409-94ba-a6965fd3afc4) <br>
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/3d5293b7-8309-48f2-9c8a-2be883ec25f3)

<br><br><br>
# kiosk main

```java
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
# controller 코드
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
    		
    		Str ing sql = "insert into orderlist_accounts" // 'orderlist_accounts' 테이블에 데이터를 삽입하는 INSERT문
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
<br><br><br>

# 관리자 로그인 코드
```java
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

# 관리자 DB조회 화면 컨트롤러
``` java
public class AdmindbController implements Initializable{
	int mcount1=0;
	int mcount2=0;
	int mcount3=0;
	int msum=0;
	
	@FXML private Button searchButton, datesearchButton, datesearch2Button, sumButton, countButton;
	@FXML private DatePicker dateDatePicker, startDatePicker, endDatePicker;
	@FXML private TextArea resultTextArea;
	@FXML private TableView<Orderlist> orderlistTableView;
	@FXML private PieChart resultPieChart;

	// <S> 각 컬럼과 데이터형식이 일치하는 자료구조를 가진 클래스 파일명	
		@FXML TableColumn<Orderlist, String> idxTableColumn;	// TableView의 TableColumn을 정의하는 코드
		@FXML TableColumn<Orderlist, String> dateTableColumn;	// String은 데이터의 타입을 명시
		@FXML TableColumn<Orderlist, String> count1TableColumn;
		@FXML TableColumn<Orderlist, String> count2TableColumn;
		@FXML TableColumn<Orderlist, String> count3TableColumn;
		@FXML TableColumn<Orderlist, String> sumTableColumn;
		// TableColumn<S, T>
		// T는 S파일에서 선언한 변수의 자료형
		
		@Override
		public void initialize(URL arg0, ResourceBundle arg1) {
			
			System.out.println("주문리스트 창이 열리고 초기화를 하려고 함");
			
			idxTableColumn.setCellValueFactory(new PropertyValueFactory<>("idx"));
			dateTableColumn.setCellValueFactory(new PropertyValueFactory<>("date"));
			count1TableColumn.setCellValueFactory(new PropertyValueFactory<>("count1"));
			count2TableColumn.setCellValueFactory(new PropertyValueFactory<>("count2"));
			count3TableColumn.setCellValueFactory(new PropertyValueFactory<>("count3"));
			sumTableColumn.setCellValueFactory(new PropertyValueFactory<>("sum"));
		}
		
		@FXML
		private void searchButtonAction(ActionEvent event) {
			// 디비 접속
			// sql을 이용해서 데이터 선택하기
			// 쿼리 실행
			mcount1=0;
			mcount2=0;
			mcount3=0;
			msum=0;
			
			DBconnect conn = new DBconnect();
			Connection conn2 = conn.getconn();
			
			// 주문리스트 테이블에 있는 자료 검색하기(정렬기준 idx)
			String sql = "select idx, to_char(order_time, 'yyyy-mm-dd hh24:mi:ss'), count1, count2, count3, sum"
					  + " from orderlist_accounts"
					  + " order by idx";
			
			try {
				PreparedStatement ps = conn2.prepareStatement(sql);
				ResultSet rs = ps.executeQuery(); // 쿼리 세팅, 실행 = Resultset에 저장
				
				ObservableList<Orderlist> datalist = FXCollections.observableArrayList();
				
				resultTextArea.setText(""); 
				while(rs.next()) {
					orderlistTableView.setItems(datalist);
					datalist.add(
								 new Orderlist(rs.getString(1), 
										 		rs.getString(2),
										 		rs.getString(3),
										 		rs.getString(4),
										 		rs.getString(5),
										 		rs.getString(6)
										 		)
								 );
					mcount1= mcount1 + Integer.parseInt(rs.getString(3));
					mcount2= mcount2 + Integer.parseInt(rs.getString(4));
					mcount3= mcount3 + Integer.parseInt(rs.getString(5));
					msum= msum + Integer.parseInt(rs.getString(6));
				}
				resultTextArea.appendText("아메리카노 : " + mcount1 + "잔\n");
				resultTextArea.appendText("카푸치노 : " + mcount2 + "잔\n");
				resultTextArea.appendText("카페라떼 : " + mcount3 + "잔\n");
				resultTextArea.appendText("총합 : " + msum);
				
				ps.close();
				rs.close();
				conn2.close(); // DB연결 끊기
			} catch (SQLException e) {
				e.printStackTrace();
			}
		}
		@FXML
		private void datesearchButtonAction(ActionEvent event) {
			mcount1=0;
			mcount2=0;
			mcount3=0;
			msum=0;	
			if(dateDatePicker.getValue()==null) {
				Alert alert = new Alert(AlertType.INFORMATION);
				alert.setTitle("알림 메시지");
				alert.setContentText("날짜를 먼저 선택하고 조회");
				alert.show();
			} else {
				DBconnect DBconn = new DBconnect();
				Connection conn2 = DBconn.getconn();
				
				String sql="select idx, order_time, count1, count2, count3, sum"
						+ " from orderlist_accounts"
						+ " where to_char(order_time, 'yyyy-mm-dd') = ?";
				try {
					PreparedStatement ps = conn2.prepareStatement(sql);
					ps.setString(1, (dateDatePicker.getValue()).toString());
					ResultSet rs = ps.executeQuery();
					ObservableList<Orderlist> datalist = FXCollections.observableArrayList();
					resultTextArea.setText("");
					while(rs.next()) {
						resultTextArea.setText("");
						datalist.add(
								 new Orderlist(rs.getString(1), 
										 		rs.getString(2),
										 		rs.getString(3),
										 		rs.getString(4),
										 		rs.getString(5),
										 		rs.getString(6)
										 		)
								 );
						mcount1= mcount1 + Integer.parseInt(rs.getString(3));
						mcount2= mcount2 + Integer.parseInt(rs.getString(4));
						mcount3= mcount3 + Integer.parseInt(rs.getString(5));
						msum= msum + Integer.parseInt(rs.getString(6));
					}
					orderlistTableView.setItems(datalist);
					resultTextArea.appendText("아메리카노 : " + mcount1 + "잔\n");
					resultTextArea.appendText("카푸치노 : " + mcount2 + "잔\n");
					resultTextArea.appendText("카페라떼 : " + mcount3 + "잔\n");
					resultTextArea.appendText("총합 : " + msum);					
						
				} catch (SQLException e) {
					e.printStackTrace();
				}	
		}		
	}
		@FXML
		private void datesearch2ButtonAction(ActionEvent event) {
			mcount1=0;
			mcount2=0;
			mcount3=0;
			msum=0;

				if(startDatePicker.getValue()==null || endDatePicker.getValue()==null) {
					Alert alert = new Alert(AlertType.INFORMATION);
					alert.setTitle("알림 메시지");
					alert.setContentText("시작 날짜와 종료 날짜 모두 입력");
					alert.show();
				} else {
					DBconnect conn = new DBconnect();	// DB 접속, 해당 기간에 일치하는 데이터 검색 sql
					Connection conn2 = conn.getconn();
					
					String sql = "select idx, order_time, count1, count2, count3, sum"
							  + " from orderlist_accounts"
							  + " where order_time >= ? and order_time <= ?"
						//	  + " where order_time between ? and ?"
							  + " order by idx";
					try {
						PreparedStatement ps = conn2.prepareStatement(sql);
						ps.setString(1, startDatePicker.getValue().toString());
						ps.setString(2, endDatePicker.getValue().plusDays(1).toString());
						ResultSet rs = ps.executeQuery();	
						resultTextArea.setText("");
						ObservableList<Orderlist> datalist = FXCollections.observableArrayList();
						// arraylist 생성
						
						while(rs.next()) {	// while문으로 arraylist에 값을 추가(add)
							datalist.add(
										 new Orderlist(
												 	   rs.getString(1),
												 	   rs.getString(2),
												 	   rs.getString(3),
												 	   rs.getString(4),
												 	   rs.getString(5),
												 	   rs.getString(6)
												 	   )
										 );
							mcount1= mcount1 + Integer.parseInt(rs.getString(3));
							mcount2= mcount2 + Integer.parseInt(rs.getString(4));
							mcount3= mcount3 + Integer.parseInt(rs.getString(5));
							msum= msum + Integer.parseInt(rs.getString(6));
						}
						resultTextArea.appendText("아메리카노 : " + mcount1 + "잔\n");
						resultTextArea.appendText("카푸치노 : " + mcount2 + "잔\n");
						resultTextArea.appendText("카페라떼 : " + mcount3 + "잔\n");
						resultTextArea.appendText("총합 : " + msum);
						
						ps.close();
						rs.close();
						conn2.close();
						orderlistTableView.setItems(datalist);
					} catch (SQLException e) {
						e.printStackTrace();
						}
					// 변수값도 계산
					// 테이블뷰에 표시하기(setItems)
					// 통계도 표시
				}			
		}
		@FXML
		private void countButtonAction (ActionEvent event) { // 판매수량 그래프 버튼을 누를 시 각 메뉴별 판매 수량을 나타낸다.
			resultPieChart.setTitle("메뉴별 판매수량 그래프");
			resultPieChart.setData(FXCollections.observableArrayList(
					new PieChart.Data("아메리카노" + mcount1 + "잔", mcount1),
					new PieChart.Data("카푸치노" + mcount2 + "잔", mcount2),
					new PieChart.Data("카페라떼" + mcount3 + "잔", mcount3)
					));
		}
		@FXML
		private void sumButtonAction (ActionEvent event) {	// 판매금액 그래프 버튼을 누를 시 총 합계를 구한다.
			resultPieChart.setTitle("메뉴별 판매금액 그래프"); // 타이틀 문자 설정
			resultPieChart.setData(FXCollections.observableArrayList( 
					new PieChart.Data("아메리카노" + mcount1*1000 + "원", mcount1*1000),
					new PieChart.Data("카푸치노" + mcount2*2000 + "원", mcount2*2000),
					new PieChart.Data("카페라떼" + mcount3*3000 + "원", mcount3*3000)
					));
		}
}
```
<br><br><br>

# Orderlist.java
``` java
public class Orderlist {
	// 실제 테이블 뷰에 들어가는 각 컬럼들을 나타낼 수 있는 자료구조를 만들어야 함
	String idx=""; 	// 각 컬럼에 해당하는 변수 선언하기
	String date="";
	String count1="";
	String count2="";
	String count3="";
	String sum="";
	
	public Orderlist(String idx, String date, String count1, String count2, String count3, String sum) {
		super();
		this.idx = idx;
		this.date = date;
		this.count1 = count1;
		this.count2 = count2;
		this.count3 = count3;
		this.sum = sum;
	}
	public String getIdx() {	// 정수형, 날짜형, 문자형이 있지만 화면에 보이는건 모두 문자형 --> 모두 String
		return idx;
	}

	public void setIdx(String idx) {
		this.idx = idx;
	}

	public String getDate() {
		return date;
	}

	public void setDate(String date) {
		this.date = date;
	}

	public String getCount1() {
		return count1;
	}

	public void setCount1(String count1) {
		this.count1 = count1;
	}

	public String getCount2() {
		return count2;
	}

	public void setCount2(String count2) {
		this.count2 = count2;
	}

	public String getCount3() {
		return count3;
	}

	public void setCount3(String count3) {
		this.count3 = count3;
	}

	public String getSum() {
		return sum;
	}

	public void setSum(String sum) {
		this.sum = sum;
	}
}
```
