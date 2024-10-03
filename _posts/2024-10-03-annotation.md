---
title: "[Java] Annotation과 Reflection"
excerpt: "Java 개발에서 어노테이션(Annotation)과 리플렉션(Reflection)은 코드의 메타데이터를 활용하여 유연하고 강력한 기능을 제공하는 중요한 개념입니다. 어노테이션과 리플렉션의 개념, 주요 기능, 그리고 두 개념의 관계와 활용 방법을 정리합니다."
categories:
  - backend
tags:
  - java
  - spring
---

# 1. Annotation(어노테이션)이란?
어노테이션이란 자바에서 코드 사이에 주석처럼 사용되며, 특별한 의미가 기능을 수행하는 `metadata` 이다. 이 메타데이터는 컴파일러에게 추가 정보를 제공하고, 소프트웨어 개발 툴이 코드의 빌드 및 배포를 자동화하도록 도와주며, 런타임시 특정 기능을 수행하도록 한다. 주로 3가지 용도로 사용된다.

## 어노테이션의 3가지 용도
1. 어노테이션을 사용해 `컴파일러가 특정 코드의 문법 에러를 체크`할 수 있다. 
	- @Override 어노테이션은 메소드가 부모 클래스의 메소드를 올바르게 오버라이딩하는지 확인한다.
2. `빌드 도구가 코드를 자동 생성하거나 코드의 일부를 수정`하는데 활용된다.
	- @Entity 어노테이션은 JPA가 데이터베이스 테이블을 생성하도록 돕는다.
3. 어노테이션은 런타임에 `리플렉션`을 사용해 특정 클래스가 메소드, 필드 등에 정보를 제공하고, 이를 바탕으로 다양한 기능을 수행한다.
 
# 2. Reflection(리플렉션)이 뭔가요?
자바의 리플렉션은 프로그램이 `실행 중에 자신이 가진 구조와 동작을 조사하고 수정`하는 기능이다. 이를 통해 컴파일 타임에 알 수 없는 클래스, 필드, 메소드 등에 접근하고, 인스턴스를 생성하거나 메소드를 호출할 수 있다.
> Reflection은 프로그래머가 데이터를 보여주고, 다른 포맷의 데이터를 처리하고, 통신을 위해 serialization(직렬화)를 수행하고, bundling을 하기 위해 일반 소프트웨어 라이브러리를 만들도록 도와준다.

## 리플렉션의 주요 기능
1. 클래스, 필드, 메소드, 생성자에 대한 `메타데이터에 접근가능`하다.
2. 클래스 이름만 알고 있다면, 실행 중에 해당 클래스의 `인스턴스를 생성`할 수 있다.
3. 클래스의 메소드 이름만 알고 있으면, 실행 중 해당  `메소드를 호출`할 수 있다.
4. 리플렉션을 사용하면 `private 접근 제어자를 가진 필드나 메소드에도 접근`할 수 있다.
	- 예시로 서드 파티 라이브러리의 private 필드 값을 변경하거나 읽을 수 있다.
		- Spring에서 BeanFactory라는 Container에서 객체가 호출되면 객체의 인스턴스를 생성하게 되는데 이 때 필요. 즉, 프레임워크에서 유연성 있는 동작을 위해 쓰인다.

## 리플렉션을 사용해 클래스의 메타데이터에 접근하기
```java
import java.lang.reflect.Field; 
import java.lang.reflect.Method;
...
// 첫 번째 방법: Class.forName("path/Person")을 통해 Class 객체 얻기 
Class<?> personClass1 = Class.forName("Person"); 
// 두 번째 방법: 인스턴스에서 getClass() 메소드 호출 
Person personInstance = new Person("Alice", 30); 
Class<?> personClass2 = personInstance.getClass(); 
// 세 번째 방법: Person.class로 Class 객체 얻기 
Class<?> personClass3 = Person.class; 

Field[] fields = personClass1.getDeclaredFields(); // 모든 필드 가져오기
// Reflection을 사용하여 필드 값 읽기 및 수정하기 
Field nameField = personClass1.getDeclaredField("name");
// private 필드에 접근할 수 있도록 설정 
nameField.setAccessible(true);
// 필드 값 수정 
nameField.set(personInstance, "Bob");
Method[] methods = personClass1.getDeclaredMethods(); // 모든 메소드 가져오기
// Reflection을 사용하여 메소드 호출하기 
Method printInfoMethod = personClass1.getDeclaredMethod("printInfo"); printInfoMethod.invoke(personInstance); // Name: Bob, Age: 25
```

## 어노테이션의 사용 순서
1. 어노테이션을 정의할 때 `@interface` 키워드를 사용한다.
```java
public @interface MyAnnotation { String value(); }
```
2. 어노테이션을 클래스, 메소드, 필드 등에 배치한다.
```java
@MyAnnotation(value="Example")
public class MyClass {
    // 클래스에 어노테이션 적용
```
3. 코드 실행 중에 리플렉션을 사용해 어노테이션 정보를 읽고, 추가적인 기능을 수행할 수 있다.
```java
//MyClass.class를 이용하여 MyClass 클래스의 Class 객체를 가져옴
Class<MyClass> obj = MyClass.class;
// MyClass클래스에 @MyAnnotation 어노테이션이 적용되었는지 확인
if (obj.isAnnotationPresent(MyAnnotation.class)) {
//MyClass 클래스에 적용된 @MyAnnotation 어노테이션 객체를 가져옴
    MyAnnotation annotation = obj.getAnnotation(MyAnnotation.class);
    System.out.println(annotation.value());
}
```


# 3. 어노테이션과 리플렉션의 관계
어노테이션은 표식만 남기고 자체적으로 동작을 수행하지 않는다. 어노테이션에 특정 동작을 부여하기 위해서는 리플렉션을 사용해 어노테이션이 적용된 요소(클래스, 메소드, 필드 등)을 확인하고, 이를 기반으로 기능을 수행하게 해야 한다.

## 일반적인 객체 생성 및 메소드 호출
```java
// Without reflection, 일반적인 방법
public class Foo { 
	public void hello() { 
		System.out.println("Hello, World!"); 
...
Foo foo = new Foo();
foo.hello();
```
- `Foo` 객체를 생성한 후 `foo.hello()`를 호출하면 `Hello, World!`라는 문자열이 출력된다.
- 이는 컴파일 타임에 `Foo` 클래스와 `hello()` 메소드를 모두 알고 있기 때문에 직접 접근하고 호출할 수 있는 것이다.

## 리플렉션을 사용한 객체 생성 및 메소드 호출
```java
// newInstance() 메소드는 Java 9 이상부터 사용이 권장되지 않으며, 대신 Constructor 객체 사용 권장
// With reflection 
Object foo = Class.forName("complete.classpath.and.Foo").newInstance();
```
- `Class.forName("complete.classpath.and.Foo")`는 `complete.classpath.and.Foo`라는 클래스의 `Class` 객체를 반환.
    - 예를 들어, 패키지가 `com.example`이고 클래스 이름이 `Foo`라면 `Class.forName("com.example.Foo")`로 클래스의 `Class` 객체를 가져온다.
- `Class` 객체의 `newInstance()` 메소드를 사용하면 해당 클래스의 인스턴스를 생성합니다. 이때 리플렉션을 사용하므로 `Foo` 클래스의 생성자에 접근할 수 있다.
> 결과적으로, `Object foo`는 `Foo` 클래스의 인스턴스를 참조하게 된다.

---
```java
Method m = foo.getClass().getDeclaredMethod("hello", new Class<?>[0]);
m.invoke(foo);
```
- `foo.getClass()`는 `foo` 객체의 클래스(`Foo`)를 반환합니다.
- `getDeclaredMethod("hello", new Class<?>[0])`는 `Foo` 클래스에 정의된 `hello`라는 이름의 메소드를 가져옵니다.
    - 두 번째 파라미터인 `new Class<?>[0]`은 `hello` 메소드의 매개변수 목록을 지정하는 것입니다. `hello()` 메소드는 매개변수를 가지지 않으므로 빈 배열을 전달합니다.
- `m.invoke(foo)`는 `foo` 객체를 대상으로 `m`이 참조하고 있는 `hello` 메소드를 호출합니다.
- 결과적으로, 리플렉션을 사용하여 `hello()` 메소드를 실행하고 `Hello, World!`를 출력하게 됩니다.

## 리플렉션을 사용해 접근제어 무시
```java
// Foo 클래스 예제
public class Foo {
    private void hello() {
        System.out.println("Hello, Private World!");
    }
}
// 리플렉션을 통한 private 메소드 호출
Foo foo = new Foo();
Method m = foo.getClass().getDeclaredMethod("hello");
m.setAccessible(true);  // private 접근을 허용
m.invoke(foo);  // 출력: "Hello, Private World!"
```
- `setAccessible(true)` 메소드를 호출하여 `hello()` 메소드에 대한 접근 권한을 설정합니다. 이를 통해 `private` 메소드나 필드에도 접근하여 값을 읽고 변경할 수 있습니다.

## 리플렉션을 사용하는 이유
- 스프링과 같은 프레임워크는 리플렉션을 사용해 객체를 동적으로 생성하고, 어노테이션을 분석해 클래스와 메소드의 역할을 동적으로 결정합니다. 
- 외부 라이브러리나 클래스의 private 필드와 메소드에 접근해 변경할 수 있습니다. 
- 실행 중에 클래스와 메소드의 정보를 확인하고 이를 기반으로 동작을 결정할 수 있습니다. 이를 통해 다양한 동적 기능을 구현할 수 있습니다.

## 리플렉션의 한계와 단점
- 일반적인 메소드 호출에 비해 성능이 떨어집니다. 리플렉션이 실행 중에 타입 체크와 메소드 탐색을 수행하기 때문입니다.
- 접근 제어자를 무시할 수 있기 때문에 보안상의 문제가 발생할 수 있습니다.
- 리플렉션을 사용하면 코드의 가독성이 떨어지고, 유지보수가 어려워집니다. 또 리플렉션은 컴파일 타임에 오류를 감지할 수 없어 런타임에 예외가 발생하기 쉬운 구조를 가집니다.

## **Spring에서의 Reflection 활용 예**
Spring은 리플렉션을 사용하여 다양한 기능을 제공합니다. 대표적인 예로는 다음과 같은 것들이 있습니다:
1. **의존성 주입 (Dependency Injection)**: `@Autowired`, `@Inject`와 같은 어노테이션을 통해 객체의 의존성을 주입할 때, 리플렉션을 사용하여 클래스와 필드 정보를 확인하고, 해당 객체를 생성 및 주입합니다.
2. **애플리케이션 컨텍스트에서의 Bean 관리**: `@Component`, `@Service`, `@Repository`와 같은 어노테이션을 통해 Spring이 관리하는 Bean 객체들을 등록하고 관리합니다. 이 과정에서 클래스 정보를 분석하고, 특정 로직을 실행하기 위해 리플렉션을 사용합니다.
3. **AOP(Aspect Oriented Programming)**: AOP는 리플렉션을 사용하여 런타임에 메소드 호출을 가로채고, 부가 기능을 수행한 후 메소드를 호출하는 기능을 제공합니다.