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
	
	
	public int getemployee_id() {
		return employee_id;
	}
	public void setemployee_id(int number) {
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
	public Date gethire_date() {
		return hire_date;
	}
	public void sethire_date(Date date) {
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

```
```jsp
-----------------------------------------------ojdbc_select.jsp-----------------------------------------------
```

