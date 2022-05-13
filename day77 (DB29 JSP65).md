# [오전수업] DB 29차
> DB 28차에 실시한 테스트 풀이 실시

---

# [오후수업] JSP 65차
## Oracle - JDBC 연동
> - ojdbc8.jar 파일 Build Path 실시
> - jsp16_oracle_jdbc 클래스 생성 후, jsp11_board 클래스에서 JdbcUtil.java 복사 -> jsp16_oracle_jdbc 클래스에 붙여넣기 실시

```java
-----------------------------------------------EmployeesBean.java-----------------------------------------------
package jsp16_oracle_jdbc;

import java.sql.Date;

public class EmployeesBean {
	private int employee_id;
	private String first_name;
	private String last_name;
	private String email;
	private String phone_number;
	private Date hire_date;
	private String job_id;
	private int salary;
	private double commission_pct;
	private int manager_id;
	private int department_id;
	
	
	public int getEmployee_id() {
		return employee_id;
	}
	public void setEmployee_id(int number) {
		this.employee_id = number;
	}
	public String getFirst_name() {
		return first_name;
	}
	public void setFirst_name(String first_name) {
		this.first_name = first_name;
	}
	public String getLast_name() {
		return last_name;
	}
	public void setLast_name(String last_name) {
		this.last_name = last_name;
	}
	public String getEmail() {
		return email;
	}
	public void setEmail(String email) {
		this.email = email;
	}
	public String getPhone_number() {
		return phone_number;
	}
	public void setPhone_number(String phone_number) {
		this.phone_number = phone_number;
	}
	public Date getHire_date() {
		return hire_date;
	}
	public void setHire_date(Date date) {
		this.hire_date = date;
	}
	public String getJob_id() {
		return job_id;
	}
	public void setJob_id(String job_id) {
		this.job_id = job_id;
	}
	public int getSalary() {
		return salary;
	}
	public void setSalary(int salary) {
		this.salary = salary;
	}
	public double getCommission_pct() {
		return commission_pct;
	}
	public void setCommission_pct(double commission_pct) {
		this.commission_pct = commission_pct;
	}
	public int getManager_id() {
		return manager_id;
	}
	public void setManager_id(int manager_id) {
		this.manager_id = manager_id;
	}
	public int getDepartment_id() {
		return department_id;
	}
	public void setDepartment_id(int department_id) {
		this.department_id = department_id;
	}
	
	@Override
	public String toString() {
		return "EmployeesBean [employee_id=" + employee_id + ", first_name=" + first_name + ", last_name=" + last_name
				+ ", email=" + email + ", phone_number=" + phone_number + ", hire_date=" + hire_date + ", job_id="
				+ job_id + ", salary=" + salary + ", commission_pct=" + commission_pct + ", manager_id=" + manager_id
				+ ", department_id=" + department_id + "]";
	}
	
	
	
}


-----------------------------------------------EmployeesDAO.java-----------------------------------------------
package jsp16_oracle_jdbc;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.ResultSetMetaData;
import java.sql.SQLException;
import java.util.ArrayList;

public class EmployeesDAO {

	public ArrayList<ArrayList> getEmployeesList() {
		// 1단계 & 2단계 작업을 수행할 getConnection() 메서드 호출
		Connection con = JdbcUtil.getConnection();

		// 컬럼명 목로과 직원 목록을 함께 저장할 ArrayList 객체 생성
		ArrayList<ArrayList> employeesInfo = new ArrayList<ArrayList>();
		PreparedStatement pstmt = null;
		ResultSet rs = null;
		
		try {
			// employees 테이블 전체 조회 후 ArrayList 객체에 직원 목록 저장
			String sql = "SELECT * FROM employees";
			pstmt = con.prepareStatement(sql);
			rs = pstmt.executeQuery();
			
			ArrayList<EmployeesBean> employeesList = new ArrayList<EmployeesBean>();
			
			while(rs.next()) {
				EmployeesBean emp = new EmployeesBean();
				emp.setEmployee_id(rs.getInt(1));
				emp.setFirst_name(rs.getString(2));
				emp.setLast_name(rs.getString(3));
				emp.setEmail(rs.getString(4));
				emp.setPhone_number(rs.getString(5));
				emp.setHire_date(rs.getDate(6));
				emp.setJob_id(rs.getString(7));
				emp.setSalary(rs.getInt(8));
				emp.setCommission_pct(rs.getInt(9));
				emp.setManager_id(rs.getInt(10));
				emp.setDepartment_id(rs.getInt(11));

//				System.out.println(emp);
				
				employeesList.add(emp);
			}
			
			// ----------------------------------------------------------------------------------
			// 조회된 테이블 정보에 대한 부가정보(= 메타데이터) 확인
			// ResultSet 객체의 getMetaData() 메서드를 호출하여 ResultSetMetaData 객체 리턴받기
			ResultSetMetaData rsmd = rs.getMetaData();
//			System.out.println("전체 컬럼 갯수 : " + rsmd.getColumnCount());
			
			// 컬럼 갯수만큼 for문을 통해 반복하면서 컬럼 정보에 접근(컬럼명, 타입 등)
			// => 주의! 배열과 달리 컬럼인덱스는 1부터 시작하므로 1 ~ 카운트까지 반복
			// => 조회된 컬럼명을 columnNameList 객체에 저장(ArrayList 타입)
			ArrayList<String> columnNameList = new ArrayList<String>();
			for(int i = 1; i <= rsmd.getColumnCount(); i++) {
				// getColumnNHame() 또는 getColumnLable() 메서드를 통해 컬럼명 가져오기
//				System.out.println(rsmd.getColumnName(i) + ", " + rsmd.getColumnLabel(i));
				
				// getColumnType() 메서드는 데이터타입값(코드값)을 가져오며,
				// getColumnTypeName() 메서드는 데이터타입이름을 가져옴
//				System.out.println(rsmd.getColumnType(i) + ", " + rsmd.getColumnTypeName(i));
//				System.out.println(rsmd.getColumnTypeName(i) + ", " + rsmd.getColumnDisplaySize(i));
				
				columnNameList.add(rsmd.getColumnName(i));
			}
			// ----------------------------------------------------------------------------------
			// 컬럼명목록과 직원목록을 다시 ArrayList 객체(employeesInfo) 에 저장
			employeesInfo.add(columnNameList);
			employeesInfo.add(employeesList);
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			JdbcUtil.close(rs);
			JdbcUtil.close(pstmt);
			JdbcUtil.close(con);
		}
		
		return employeesInfo;
		
	}
}

```
```jsp
-----------------------------------------------ojdbc_select.jsp-----------------------------------------------
<%@page import="jsp16_oracle_jdbc.EmployeesBean"%>
<%@page import="java.util.ArrayList"%>
<%@page import="jsp16_oracle_jdbc.EmployeesDAO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>직원 리스트</h1>
	<%
	EmployeesDAO dao = new EmployeesDAO();
	ArrayList<ArrayList> employeesInfo = dao.getEmployeesList();
	
	// 컬럼명 목록 꺼내서 저장
	ArrayList<String> columnNameList = employeesInfo.get(0);
	// 직원 목록 꺼내서 저장
	ArrayList<EmployeesBean> employeesList = employeesInfo.get(1);
	%>
	<table border="1">
		<tr>
		<%for(String columnName : columnNameList) { %>
			<th><%=columnName %></th>
		<%} %>
		</tr>
		
		<%for(EmployeesBean employees : employeesList) { %>
			<tr>
				<td><%=employees.getEmployee_id() %></td>	
				<td><%=employees.getFirst_name() %></td>	
				<td><%=employees.getLast_name() %></td>	
				<td><%=employees.getEmail() %></td>	
				<td><%=employees.getPhone_number() %></td>	
				<td><%=employees.getHire_date() %></td>	
				<td><%=employees.getJob_id() %></td>	
				<td><%=employees.getSalary() %></td>	
				<td><%=employees.getCommission_pct() %></td>	
				<td><%=employees.getManager_id() %></td>	
				<td><%=employees.getDepartment_id() %></td>			
			</tr>																																						
		<%} %>
	</table>
</body>
</html>
```

