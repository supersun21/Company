# Company 클래스(main)

* Employee[] emps와 empCnt를 필드로 가지는 메인 클래스

```java
import emp.Employee;
import emp.IBusinessTrip;
import emp.PartTime;
import emp.Permanent;
import emp.Sales;

public class Company {
	Employee[] emps = new Employee[100];
	int empCnt;
	
	public void addEmployee(Employee emp) { //upcasting(자식타입을 부모타입에)
		emps[empCnt++] = emp;
	}
	
	public void empList() {
		for(int i = 0; i < empCnt; i++)
		{
			System.out.println(emps[i].info());
		}
	}
	
	public int getTotEmpPay() {
		int tot = 0;
		for(int i = 0; i < empCnt; i++)
		{
			tot += emps[i].getPay();
		}
		return tot;
	}
	
	public void regBusinessTrip(IBusinessTrip emp, int day) {
		emp.goBusinessTrip(day);
	}

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//Employee e = new Employee("100", "나직원"); //추상클래스는 객체 생성 불가능
		
		Company com = new Company();
		Permanent e1 = new Permanent("101", "김과장", 4000000);
		Sales e2 = new Sales("102", "박대리", 1000000, 1000000);
		PartTime e3 = new PartTime("103", "홍차장", 160, 50000);
		com.addEmployee(e1);
		com.addEmployee(e2);
		com.addEmployee(e3);
		
		//com.regBusinessTrip(e1, 3); //불가능하게
		com.regBusinessTrip(e2, 2); //가능하게
		com.regBusinessTrip(e3, 3); //가능하게
		
		com.empList();
		System.out.println("총 급여액 : " + com.getTotEmpPay());
	}

}
```



# Employee 클래스

* id와 name을 필드로 가지는 클래스

```java
package emp;

public abstract class Employee {
	String id;
	String name;
	
	public Employee(String id, String name) {
		this.id = id;
		this.name = name;
	}
	
	public String info() {
		return "사번 : "+id+", 이름 : "+name;
	}
	
	abstract public int getPay();
}
```



# Permanent 클래스

* Employee 클래스를 상속받아 pay를 필드로 가지는 클래스

```java
package emp;

public class Permanent extends Employee {
	int pay;
	
	public Permanent(String id, String name, int pay) {
		super(id, name);
		this.pay = pay;
	}

	public int getPay() {
		return pay;
	}

	@Override
	public String info() {
		// TODO Auto-generated method stub
		return super.info() +  ", 급여 : " + getPay();
	}
}
```



# Sales 클래스

* Permanent 클래스를 상속받고 IBusinessTrip 인터페이스를 구현하며 incentive를 필드로 가지는 클래스

```java
package emp;

public class Sales extends Permanent implements IBusinessTrip {
	int incentive;
	
	public Sales(String id, String name, int pay, int incentive) {
		super(id, name, pay);
		this.incentive = incentive;
	}	

	public int getIncentive() {
		return incentive;
	}

	public void setIncentive(int incentive) {
		this.incentive = incentive;
	}

	@Override
	public int getPay() {
		// TODO Auto-generated method stub
		return super.getPay() + getIncentive();
	}

	@Override
	public void goBusinessTrip(int day) {
		// TODO Auto-generated method stub
		incentive += day * 100000;
	}	
}
```



# PartTime 클래스

* Employee 클래스를 상속하며 IBusinessTrip 인터페이스를 구현하며 time과 payPerTime을 필드로 가지는 클래스

```java
package emp;

public class PartTime extends Employee implements IBusinessTrip {
	int time;
	int payPerTime;
	
	public PartTime(String id, String name, int time, int payPerTime) {
		super(id, name);
		this.time = time;
		this.payPerTime = payPerTime;
	}	
	
	public int getPay() {
		return time * payPerTime;
	}

	@Override
	public String info() {
		// TODO Auto-generated method stub
		return super.info() + ", 급여 : " + getPay();
	}

	@Override
	public void goBusinessTrip(int day) {
		// TODO Auto-generated method stub
		time += day * 24;
	}
	
}
```



# IBusinessTrip 인터페이스

* Company 클래스의 regBusinessTrip 메소드를 구현하기 위한 인터페이스

```java
package emp;

public interface IBusinessTrip {
	void goBusinessTrip(int day);
}
```

