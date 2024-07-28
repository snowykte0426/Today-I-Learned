# Spring MVC 패턴


### 개요
---
 + Spring 프레임워크(freamwork)의 모듈 중에서 웹 계층에 서블릿(Servlet) API를 기반으로 클라이언트의 요청을 처리하는 모듈을 스프링 웹 MVC(Spring-Web-MVC) 또는 스프링 MVC(Spring MVC)라 한다.

<h4><span class="rotated_star">&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp★</span> <b>서블릿(Servlet)이란?</b></h4>
<blockquote>
    <ul>
        <li>클라이언트의 요청을 처리하도록 특정한 규약에 맞추어 <b>Java</b> 코드로 작성하는 클래스 파일이다.</li>
        <li>대표적으로 <b>아파치 톰캣(Apache Tomcat)</b>은 이런 서블릿들이 웹 애플리케이션으로 동작하도록 도와주는 <b>서블릿 컨테이너(Servlet Container)</b> 중 하나이다.</li>
        <li>Spring MVC 내부에선 서블릿을 기반으로 웹 애플리케이션을 동작하며 <b>스프링부트(Springboot)</b>는 기본적으로 아파치 톰캣을 <b>내장</b>하고 있다.</li>
    </ul>
</blockquote>


### MVC란?
---
 + MVC는 **M**odel, **V**iew, **C**ontroller의 약자이다.
 + 애플리케이션을 개발할 때 사용하는 디자인 패턴으로 개발 영역을 Model,View,Controller로 구분하여 각 역할에 맞게 코드를 작성하는 개발 방식이다.
   ### Model
   + 처리된 작업의 결과 테이터를 요청한 클라이언트에게 돌려주는데,이때 **클라이언트에게 돌려주는 작업 처리 결과**를 Model이라고 한다.
   ### View
   + Model를 이용하여 웹 브라우저 등의 애플리케이션의 화면에 보이는 리소스(Resource)를 제공하는 것을 말한다.
   + Spring MVC에는 다양한 View 기술이 존재한다.
     + HTML 페이지 출력
     + PDF, Excel 등 문서 형태의 출력
     + XML, JSON 등 특정 형식의 포맷으로 변환
    ### Controller
    + Controller는 클라이언트의 요청을 직접적으로 전달받는 <b>엔드포인트(Endpoint)</b>로써 Model과 View의 징검다리 역할을 한다.
    + 클라이언트 측의 요청을 전달받아 비즈니스 로직(Business logic)을 거친 후, 만들어진 Model 데이터를 View로 전달하는 역할이다.
### Spring MVC의 구성과 동작
![Spring MVC](https://github.com/snowykte0426/TIL/blob/main/img/Spring_MVC.png?raw=true)
+ 먼저 가장 앞에 보이는 **DispatcherServlet**은 **HttpServlet**를 상속받아 사용한다.
+ 서블릿이 호출되면 HttpServlet가 제공하는 <b>Service()</b>가 호출되며 Spring MVC는 <b>FreamworkServlet.service()</b>를 시작으로 여러 메서드(method)가 호출되며 <b>DispacherServlet.doDispatch()</b>가 최종적으로 호출된다.
+ 기본적으로 이러한 형태로 동작된다.
  + **핸들러(Handler)** 조회: 핸들러 매핑을 통해 URL에 매핑된 핸들러를 조회한다.
  + 핸들러 어댑터 조회: 핸들러를 실행할 수 있는 핸들러 어댑터를 조회한다.
  + 핸들러 어댑터 실행: 핸들러 어댑터를 실행한다.
  + **ModelAndView** 반환: 핸들러가 반환하는 정보를 핸들러 어댑터가 ModelAndView로 변환하여 반환한다.
  + **ViewResolver** 호출: 뷰 리졸버를 찾고 실행한다.
  + View 반환: 뷰 리졸버가 뷰(View)의 논리 이름을 물리 이름으로 바꾸고, 렌더링 역할을 담당하는 뷰 객체를 반환한다.
  + View 렌더링: 뷰를 통해 뷰를 렌더링 한다.
  ### 인터페이스
  + Spring MVC의 장점 중 하나로 DispatcherServlet 코드의 변경 없이 원하는 기능을 변경하고 확장 할 수 있다는 것이다.
  + 동작의 대부분을 인터페이스로 제공한다.
    + 핸들러 매핑
        ```java
            org.springframework.web.servlet.HandlerMapping
        ```
    + 핸들러 어댑터
         ```java
            org.springframework.web.servlet.HandlerAdapter
        ```
    + 뷰 리졸버
         ```java
            org.springframework.web.servlet.ViewResolver
        ```
    + 뷰
         ```java
            org.springframework.web.servlet.View
        ```