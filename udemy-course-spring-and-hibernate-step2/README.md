# udemy-course-spring-and-hibernate

Step 2 :Spring Dependency Injection - XML Configuration
---

I. Constructor Injection

Summary : 
- Define the dependency interface and class
- Create a constructor in your class for injections
- Configure the dependency injection in Spring config file 

A. Define the dependency interface and class

1. Create new interface FortuneService

```java
public interface FortuneService {
	
	public String getFortune();

}
```

2. Create a class which implements the interface

```java
public class HappyFortuneService implements FortuneService {

	@Override
	public String getFortune() {
		return "Today is your lucky day!";
	}

}
```

3. Update Coach interface

```java
public interface Coach {
	
	public String getDailyWorkout();
	
	public String getDailyFortune();

}
```

B. Create a constructor in your class for injections

1. Update BaseballCoach 

```java
public class BaseballCoach implements Coach{
	
	// define a private field for the dependency (new)
	private FortuneService fortuneService; 
	
	// defined a constructor for dependency injection (new)
	public BaseballCoach(FortuneService theFortuneService){
		fortuneService = theFortuneService;
	}
	
	@Override
	public String getDailyWorkout(){
		return "Spend 30 minutes on batting practice";
	}

	@Override
	public String getDailyFortune() {
		// TODO Auto-generated method stub
		return null;
	}

}
```

2. Update getDailyFortune() in BaseballCoach

```java
public class BaseballCoach implements Coach{
	
	// define a private field for the dependency
	private FortuneService fortuneService;
	
	// defined a constructor for dependency injection
	public BaseballCoach(FortuneService theFortuneService){
		fortuneService = theFortuneService;
	}
	
	@Override
	public String getDailyWorkout(){
		return "Spend 30 minutes on batting practice";
	}

	@Override
	public String getDailyFortune() { // (update)
		// use my fortuneService to get a fortune
		return fortuneService.getFortune();
	}

}
```

3. Configure the dependency injection in Spring config file

Update applicationContext

```xml
	<!--  define the dependency -->
	<bean id="myFortune"
		class="com.jvanhouteghem.springdemo.HappyFortuneService">
	</bean>

    <!-- Define your beans here -->
    <bean id="myCoach" 
    	class="com.jvanhouteghem.springdemo.BaseballCoach">
    	<!--  set up constructor injection -->
    	<constructor-arg ref="myFortune"/>
    </bean>
```

4. Update HelloSpringApp

```java
public class HelloSpringApp {

	public static void main(String[] args) {
		
		// load the spring configuration file
		ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
		
		// retrieve bean from spring container
		Coach theCoach = context.getBean("myCoach", Coach.class); // the bean id
		
		// call methods on the bean
		System.out.println(theCoach.getDailyWorkout());
		
		// let's call our new method for fortunes (new)
		System.out.println(theCoach.getDailyFortune());
		// close the context
		context.close();
	}

}
```

Output : 
```
>>> Spend 30 minutes on batting practice
>>> Today is your lucky day!
```
