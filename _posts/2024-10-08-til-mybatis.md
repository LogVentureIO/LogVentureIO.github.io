---
title: "[TIL] 7주차 ⎮ BE - Spring 구조 ➕ MyBatis ➕ JSP"
excerpt: "Spring 프레임워크의 구조와 MyBatis를 사용한 CRUD 구현을 다룹니다. DTO와 DAO의 차이, 서비스와 컨트롤러의 역할, MyBatis를 사용하는 이유와 함께 MyBatis와 JSP를 활용한 CRUD 예제를 정리합니다."
categories:
  - blog
  - backend
tags:
  - spring
  - DB
  - TIL/WIL
---
# 1. Spring 구조
![image](https://github.com/user-attachments/assets/404439d8-42f3-41ea-8f53-bde6ec670deb)

스프링 부트에서 백엔드 애플리케이션을 개발할 때 자주 사용하는 구조는 **MVC 패턴**과 **계층형 아키텍처**를 기반으로 합니다. 이 구조는 일반적으로 `브라우저`에서 시작하여 `컨트롤러`, `서비스`, `DAO`, `DB`로 이어집니다. 여기에 `DTO`와 `엔티티(Entity)`가 각 계층 간 데이터를 전달할 때 사용됩니다.

**예제**
- 클라이언트(Browser)에서 /users/1로 GET 요청을 보냅니다.
- **Controller**가 요청을 받아서 `UserService`의 `getUserById()` 메소드를 호출합니다.
- **Service**는 `UserRepository`를 이용하여 해당 ID의 `User` 엔티티를 데이터베이스에서 가져옵니다.
- **DAO(UserRepository)**는 데이터베이스에서 `User` 엔티티를 찾고, 결과를 서비스로 반환합니다.
- **Service**는 이 엔티티를 `UserDTO`로 변환하여 컨트롤러로 반환합니다.
- **Controller**는 이 DTO를 클라이언트에 JSON 형태로 응답합니다.4

## DTO (Data Transfer Object)
`DTO`는 **특정 계층 간에 필요한 데이터만 전달하기 위해 사용하는 객체**입니다. 이를 통해 필요한 필드만 포함하고, 불필요한 필드나 정보를 생략함으로써 데이터 전송을 최적화하고 코드의 가독성과 유지보수성을 높입니다.

### 🐣 DAO와의 차이점?
- DAO는 데이터베이스와의 상호작용을 담당하는 객체로, 데이터베이스의 전체 필드를 관리하고 CRUD 작업을 수행합니다.
- DAO는 데이터베이스와 직접적으로 연관되므로, 테이블의 모든 필드를 가져오거나, 특정 쿼리를 실행하여 필터링된 결과를 반환할 수 있습니다.
- DTO는 계층 간 데이터 전달을 목적으로 하며, 특정 요청이나 응답에 필요한 `일부 필드만 포함`합니다.
- DAO가 반환한 `Entity`나 서비스 계층에서 만든 데이터를 필터링하여 필요한 부분만 DTO에 담아 전달합니다.
- DTO는 데이터베이스 테이블과 직접적인 연관이 없으며, 특정 목적에 맞게 데이터를 구조화할 수 있습니다.

## Service에서는 무엇을 하나요?
서비스(Service) 단의 역할은 **비즈니스 로직**을 처리하는 것입니다. 컨트롤러가 요청을 받아서 `데이터를 어떻게 처리할지 결정`하는 부분이 서비스 단입니다. 여기서 **비즈니스 로직**은 단순히 데이터베이스에서 데이터를 가져오고 반환하는 것을 넘어서, 데이터 처리 과정에서 필요한 여러 가지 추가 작업을 의미합니다.

### 🐣 Controller과 어떻게 다른가요?
컨트롤러와 서비스의 역할을 명확히 구분해야 합니다.
- 컨트롤러는 주로 **사용자의 요청을 처리하고 응답을 반환**하는 역할을 하며, 서비스는 **비즈니스 로직**을 처리하는 데 초점을 맞춥니다.
- 컨트롤러는 일종의 `"입구 역할"`로, 사용자가 보내는 HTTP 요청을 받아 `적절한 서비스로 연결`하는 창구역할을 합니다. 컨트롤러는 주로 **요청을 분배하고** 서비스에서 처리된 결과를 다시 **클라이언트에게 응답**하는 간단한 중계자 역할을 담당하므로, 로직 자체는 많이 포함되지 않습니다.


# 2. Mybatis
![image](https://github.com/user-attachments/assets/f2d87730-5d0a-4a4d-a1d0-cab3aa45a346)

[참고](https://www.elancer.co.kr/blog/detail/231?seq=231)
백엔드에서 데이터를 저장하고 조회하려면 데이터베이스를 활용해야 하는데, 백엔드에서 데이터베이스를 사용하는 프레임워크로 가장 많이 쓰이는 기술이 `Mybatis`와 `JPA`입니다.
- 데이터 베이스 접속을 편하게 사용하기 위해 SQL Mapper 기술과, ORM(Object Relational Mapping) 기술을 제공합니다. 둘 다 DB와의 연동, 저장을 위한 기술이며, SQL Mapper는 ‘**개발자가 작성한 SQL 실행 결과를 객체에 매핑**’시켜주는 프레임워크이며, ORM은 객체와 DB의 데이터를 ‘**자동으로 매핑**’시켜주는 프레임워크를 말합니다.

SQL Mapper 기술을 제공하는 것이 MyBatis이며, ORM 기술을 제공하는 것이 `JPA(Java Persistence Api`입니다. 두 가지 기술은 모두 데이터를 관계형 데이터베이스에 저장, 즉 영속화(Persistence) 시킨다는 측면에서는 동일하지만, 서로 다른 접근 방식을 채택하고 있습니다.

## MyBatis를 왜 사용하나요?
MyBatis는 Java 애플리케이션에서 **SQL을 더 효과적으로 관리**하고 객체-관계 매핑(ORM)을 제공하기 위해 사용되는 프레임워크입니다.
- `SQL을 개발자가 직접 제어`할 수 있어 복잡한 쿼리나 조건문 작성에 편리합니다. 또 SQL문을 XML 파일로 분리해 작성할 수 있어 SQL의 재사용성과 유지보수성이 높아집니다.
- MyBatis는 SQL 쿼리를 XML 파일에 작성하여 `SQL 코드와 자바 비즈니스 로직을 분리`할 수 있습니다. 이는 SQL 수정이 필요할 때 자바 코드에 영향을 주지 않으므로 유지보수에 유리합니다.

## MyBatis는 언제 사용하나요? (JPA 웨않써)
JPA는 객체 지향 모델에 기반한 ORM 툴로 SQL을 자동으로 생성하고 관리하므로 SQL 쿼리를 직접 작성하지 않아도 됩니다. 자바 클래스로 데이터베이스 테이블 간의 매핑을 자동으로 처리합니다.
- MyBatis는 복잡한 SQL을 최적화할 수 있어 성능에 민감한 경우 유리할 수 있지만, 개발자가 쿼리를 직접 작성해야 하므로 코드 관리가 어렵다. 
- JPA는 기본적으로 캐싱 및 트랜잭션 관리를 제공해 성능을 향상시킬 수 있지만, 직접 SQL 문을 작성하는 것이 아니라 최적화가 필요한 경우 덜 유연하다. 자동화된 CRUD 처리가 필요하다면 좋다.

# 3. JSP와 MyBatis 사용 CRUD
전체구조
```markdown
src/
|-- main/
    |-- java/
    |   |-- com.example/
    |       |-- controller/
    |           |-- UserController.java
    |       |-- service/
    |           |-- UserService.java
    |       |-- mapper/
    |           |-- UserMapper.java
    |       |-- model/
    |           |-- User.java
    |-- resources/
        |-- mappers/
        |   |-- UserMapper.xml
        |-- templates/
            |-- userList.jsp
            |-- userDetail.jsp

```
## 1.  application.properties 설정
MySQL 데이터베이스 설정 및 MyBatis의 `Mapper XML` 위치를 지정
```bash
spring.datasource.url=jdbc:mysql://localhost:3306/mydatabase
spring.datasource.username=root 
spring.datasource.password=yourpassword 
mybatis.mapper-locations=classpath:mappers/*.xml
```
1. MySQL 데이터베이스의 URL을 지정합니다. `localhost`와 `3306`은 기본 호스트와 포트 번호(mysql)입니다.
2. 데이터베이스에 접근할 사용자명을 지정합니다.
3. 데이터베이스에 접근할 비밀번호를 지정합니다.
4. MyBatis의 XML Mapper 파일들이 위치한 경로를 지정합니다. 
	- `classpath:mappers/*.xml`로 설정하여, `src/main/resources/mappers/` 폴더 안에 있는 모든 XML 파일을 찾을 수 있도록 합니다.


## 2. 엔티티 클래스
User 클래스는 데이터베이스 테이블의 데이터를 매핑하기 위한 간단한 POJO 클래스
```java
import lombok.Data;
@Data // lombok 어노테이션 게터,세터 등 자동 생성
public class User {
    private Long id;
    private String name;
    private String email;
}
```
+. `POJO`(Plain Old Java Object) 순수 자바 객체, 일반적인 자바 클래스

## 3. MyBatis Mapper 인터페이스와 XML 파일 작성
MyBatis를 사용하여 SQL 문을 작성할 수 있는 `Mapper` 인터페이스와 `XML` 파일을 작성
> MyBatis를 사용할 때 간단한 SQL은 아래처럼 인터페이스에 어노테이션을 사용해 작성할 수도 있다. 

### 3.1. **UserMapper.java**: MyBatis Mapper 인터페이스
먼저, `UserMapper` 인터페이스를 작성합니다. MyBatis는 이 인터페이스와 관련된 SQL을 `xml` 파일에서 찾아서 실행합니다.
```java
// UserMapper.java
import org.apache.ibatis.annotations.Mapper;
import java.util.List;

@Mapper
public interface UserMapper {
    User findById(Long id);
    List<User> findAll();
    void insert(User user);
    void update(User user);
    void delete(Long id);
}

```
아래처럼 바로 어노테이션을 사용할 수도 있다.
```java
// UserMapper.java 어노테이션 SQL
import org.apache.ibatis.annotations.*;
import java.util.List;
@Mapper
public interface UserMapper {
    @Select("SELECT * FROM users WHERE id = #{id}")
    User findById(Long id);
    @Select("SELECT * FROM users")
    List<User> findAll();
    @Insert("INSERT INTO users(name, email) VALUES(#{name}, #{email})")
    void insert(User user);
    @Update("UPDATE users SET name=#{name}, email=#{email} WHERE id=#{id}")
    void update(User user);
    @Delete("DELETE FROM users WHERE id=#{id}")
    void delete(Long id);
}
```

### 3.2. MyBatis XML Mapper 파일
`src/main/resources/mappers/UserMapper.xml`
- **namespace**: 이 XML 파일이 연결될 인터페이스를 지정합니다. `UserMapper` 인터페이스와 연결되도록 합니다.

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.example.mapper.UserMapper">
    <!-- 사용자 정보 조회 -->
    <select id="findById" resultType="com.example.model.User">
        SELECT * FROM users WHERE id = #{id}
    </select>
    <!-- 모든 사용자 목록 조회 -->
    <select id="findAll" resultType="com.example.model.User">
        SELECT * FROM users
    </select>
    <!-- 사용자 정보 삽입 -->
    <insert id="insert" parameterType="com.example.model.User">
        INSERT INTO users(name, email) VALUES(#{name}, #{email})
    </insert>
    <!-- 사용자 정보 업데이트 -->
    <update id="update" parameterType="com.example.model.User">
        UPDATE users SET name=#{name}, email=#{email} WHERE id=#{id}
    </update>

    <!-- 사용자 정보 삭제 -->
    <delete id="delete" parameterType="Long">
        DELETE FROM users WHERE id=#{id}
    </delete>
</mapper>

```

## 4. 서비스 클래스
비즈니스 로직 처리
```java
import org.springframework.stereotype.Service;
import org.springframework.beans.factory.annotation.Autowired;
import java.util.List;

@Service
public class UserService {
    @Autowired
    private UserMapper userMapper;
    
    public User findUserById(Long id) {
        return userMapper.findById(id);
    }
    public List<User> findAllUsers() {
        return userMapper.findAll();
    }
    public void addUser(User user) {
        userMapper.insert(user);
    }
    public void updateUser(User user) {
        userMapper.update(user);
    }
    public void deleteUser(Long id) {
        userMapper.delete(id);
    }
}
```

## 5. 컨트롤러 클래스
사용자 요청 처리, JSP 페이지로 데이터 전달
```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;

@Controller
@RequestMapping("/users")
public class UserController {
    @Autowired
    private UserService userService;

    @GetMapping("/{id}")
    public String getUserById(@PathVariable Long id, Model model) {
        User user = userService.findUserById(id);
        model.addAttribute("user", user);
        return "userDetail"; // JSP 파일로 이동
    }
    @GetMapping
    public String getAllUsers(Model model) {
        model.addAttribute("users", userService.findAllUsers());
        return "userList"; // JSP 파일로 이동
    }
    @PostMapping
    public String addUser(@ModelAttribute User user) {
        userService.addUser(user);
        return "redirect:/users";
    }
    @PostMapping("/update")
    public String updateUser(@ModelAttribute User user) {
        userService.updateUser(user);
        return "redirect:/users";
    }
    @PostMapping("/delete/{id}")
    public String deleteUser(@PathVariable Long id) {
        userService.deleteUser(id);
        return "redirect:/users";
    }
}
```

## 6. JSP 뷰
사용자 화면 View

