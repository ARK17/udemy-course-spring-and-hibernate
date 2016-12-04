# udemy-course-spring-and-hibernate

Step 4 : Spring Inversion of Control - Java Annotations
---

What are java annotations ?
- Special labels/markers added to java classes
- Provide meta-data about the class 
- Processed at compile time or run-time for special processing

3 steps :
- Enable component scanning in Spring config file
- Add the @Component Annotation to your Java classes
- Retrieve bean from Spring container

I. Explicit Component Names
---

A. Enable component scanning in Spring config file

Let's start by create new package com.jvanhouteghem.springdemoannotations for spring annotations
and a new applicationContext file named applicationContextAnnotations.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd">

	<!--  add entry to enable component scanning -->
  	<context:component-scan base-package="com.jvanhouteghem.springdemoannotations"/>
    
</beans>
```

B. Add the @Component Annotation to your Java classes

1) Create Coach2 interface

```java
public interface CoachAnnotations {
	
	public String getDailyWorkout();

}
```

2) Create new TennisCoach class and add the @Component 

```java
import org.springframework.stereotype.Component;

@Component("thatSillyCoach")
public class TennisCoach implements CoachAnnotations {

	@Override
	public String getDailyWorkout() {
		return "Practice your backhand volley";
	}

}
```

NB : Spring will automatically register this bean

C. Retrieve bean from Spring container

Create new class : 

```java
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class AnnotationDemoApp {

	public static void main(String[] args) {
		
		// read spring config file
		ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("applicationContextAnnotations.xml");
		
		// get the bean from spring container
		CoachAnnotations theCoach = context.getBean("thatSillyCoach", CoachAnnotations.class);
		
		// call a method on the bean
		System.out.println(theCoach.getDailyWorkout());
		
		// close the context
		context.close();
	}

}
```

Output : 

```
>>> INFOS: Loading XML bean definitions from class path resource [applicationContextAnnotations.xml]
>>> Practice your backhand volley
```

II. Default Component Names
---

Spring also supports default bean IDs which is the class name with first letter in lower-case.

TennisCoach (the class name) --> tennisCoach (default bean ID)

A. Update TennisCoach by removing @Component("thatSillyCoach")

```java
package com.jvanhouteghem.springdemoannotations;

import org.springframework.stereotype.Component;

//@Component("thatSillyCoach") 
// Now this use the default bean id "tennisCoach"
public class TennisCoach implements CoachAnnotations {

	@Override
	public String getDailyWorkout() {
		return "Practice your backhand volley";
	}

}
```

B. Retrieve the bean - Update AnnotationDemoApp

```java
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class AnnotationDemoApp {

	public static void main(String[] args) {
		
		// read spring config file
		ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("applicationContextAnnotations.xml");
		
		// get the bean from spring container (update)
		CoachAnnotations theCoach = context.getBean("tennisCoach", CoachAnnotations.class);
		
		// call a method on the bean
		System.out.println(theCoach.getDailyWorkout());
		
		// close the context
		context.close();
	}

}
```







