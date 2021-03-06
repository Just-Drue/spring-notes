# Spring

`Java servlet` is a Java software component that extends the capabilities of a server. Although servlets can respond to any types of requests, they most commonly implement web containers for hosting web applications on web servers and thus qualify as a server-side servlet web API.

`WAR file` (Web Application Resource or Web application ARchive) is a file used to distribute a collection of JAR-files, JavaServer Pages, Java Servlets, Java classes, XML files, tag libraries, static web pages (HTML and related files) and other resources that together constitute a web

## Inversion of Control

`The approach of outsourcing the construction and management of objects.`

Explain it to me like I'm a five year old - 
```
    When you go and get things out of the refrigerator for yourself, you can cause problems. You might leave the door open, you might get something Mommy or Daddy doesn't want you to have. You might even be looking for something we don't even have or which has expired.

    What you should be doing is stating a need, "I need something to drink with lunch," and then we will make sure you have something when you sit down to eat.
```

**Source:** https://stackoverflow.com/questions/1638919/how-to-explain-dependency-injection-to-a-5-year-old

```java
public class MyApp {
    public static void main(String[] args){
        Coach bCoach = new BaseballCoach();
        System.out.println(bCoach.getDailyWorkout());
    }
}

public interface Coach {
    public String getDailyWorkout();
}

public class BaseballCoach implements Coach{
    @Override
    public String getDailyWorkout() {
        return "Spend 30 minutes on batting practice";
    }
}
```
---

## Dependency Injection

Any application is composed of many objects that collaborate with each other to perform some useful stuff. Traditionally each object is responsible for obtaining its own references to the dependent objects (dependencies) it collaborate with. This leads to highly coupled classes and hard-to-test code.

For example, consider a `Car object`.

A Car depends on `wheels, engine, fuel, battery, etc`. to run. Traditionally we define the brand of such dependent objects along with the definition of the Car object.

Without Dependency Injection (DI):

```java
class Car{
  private Wheel wh = new NepaliRubberWheel();
  private Battery bt = new ExcideBattery();
}
```

Here, the `Car` object is responsible for creating the dependent objects.

What if we want to change the `type` of its dependent object - say Wheel - after the initial `NepaliRubberWheel()` punctures? We need to recreate the Car object with its new dependency say ChineseRubberWheel(), but only the Car manufacturer can do that.

Then what does the Dependency Injection do us for...?

When using dependency injection, `objects are given their dependencies at run time rather than compile time` (car manufacturing time). So that we can now change the Wheel whenever we want. Here, the dependency (wheel) can be injected into Car at run time.

After using dependency injection:

Here, we are injecting the dependencies (Wheel and Battery) at runtime. Hence the term : Dependency Injection.

```java
class Car{
  private Wheel wh;
  private Battery bt;

  Car(Wheel wh,Battery bt) {
      this.wh = wh;
      this.bt = bt;
  }

  //Or we can have setters
  void setWheel(Wheel wh) {
      this.wh = wh;
  }
}
```
---

## Spring Bean

A `Spring Bean` is simply a Java object.

When Java objects are created by the Spring Container, then Spring refers to them as "Spring Beans".

Spring Beans are created from normal Java classes .... just like Java objects.

Here's a blurb from the Spring Reference Manual -
```
In Spring, the objects that form the backbone of your application and that are managed by the Spring IoC container are called beans. A bean is an object that is instantiated, assembled, and otherwise managed by a Spring IoC container.
```

In the early days, there was a term called "Java Beans". Spring Beans have a similar concept but Spring Beans do not follow all of the rigorous requirements of Java Beans.

`In summary, whenever you see "Spring Bean", just think Java object.`

---

## Spring Container

`Container` is used to describe any component that can contain other components inside itself. <br/> Primary functions -
* Crate and manage objects (IoC)
* Inject object's dependencies (DI)

**Special Note about init and destroy Method Signatures**

`Access modifier` - the method can have any access modifier (public, protected, private)

`Return type` - the method can have any return type. However, "void' is most commonly used. If you give a return type just note that you will not be able to capture the return value. As a result, "void" is commonly used.

`Method name` - the method can have any method name.

`Arguments` - the method can not accept any arguments. The method should be no-arg.

## Scope

By default `Bean's` scope is a singleton but we can we can define it into multiple different ones:

* `prototype` - creates a new copy, but Spring does not call the destroy method for prototype scoped beans
* `request` - each and every HTTP request will have its own instance of a bean created off the back of a single bean definition
* `session` - lifecycle of a HTTP session
* `global session` - lifecycle of a global HTTP session

```xml
<bean id="trackCoach" class="eu.justdrue.springdemo.TrackCoach" scope="prototype">
    <constructor-arg ref="myFortune" />
</bean>
```
---

## Java Annotations

`Java Annotations` are special labels / markers  added to Java classes, they provide meta-data about the class.

Useful because:
* XML configurations can be verbose
* Configure your Spring beans with Annotations
* Annotations minimizes the XML configuration

Process:
1. Enable component scanning in Spring config file
2. Add `@Component` Annotation to the Java class
3. Retrieve bean from Spring container 

```java
@Componenet("beanID")
public class SomeRandomClass implements IAFace {
    @Override
    public String doSomething() {
        return "random text";
    }
}
```

Spring can also automatically create a `Default Bean ID`, which is going to be the same as class name but with the lower first letter, e.g. TennisCoach -> tennisCoach.


There are three types of configurations -
* Bootstrapping @Configuration classes
    ```java
    @Configuration
    @ComponentScan("com.acme.app.services")
    public class AppConfig {
        // various @Bean definitions ...
    }
    ```
* Via Spring XML
    ```xml
    <beans>
        <context:annotation-config/>
        <bean class="com.acme.AppConfig"/>
    </beans>
    ```
* Via component scanning
    ```java
    @Component
    public class RandomService implements FortuneService {
        @Override
        public String getFortune() {
            return "RANDOM SERVICE";
        }
    }
    ```
---

## Spring MVC

* Framework for building web applications in Java
* Based on Model-View-Controller design pattern
* Leverages features of the Core Spring Framework (IoC, DI)

---