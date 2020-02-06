--- 
published: true
title: 간단 rest api 서버 구축
layout: post
author: sw23blues
category: server
tags: 
- spring
- mybatis
- mysql
- tomcat

---

app 만들다보니 필요해서 간단하게 rest api 서버를 구축해보았다.<br>
설치부터 서버구축까지 정리하며!<br>
안정적인 버전이 있겠지만, 알 수 없으니 무조건 최신버전으로 설치했다.<br>
쭉쭉 설치하고 프로젝트 생성전에 셋팅 하는걸로..<br>

**환경**<br>
macOS Mojave 10.14.5<br>
Java JDK 1.8.0_241<br>
Apache Tomcat 8.5.50<br>
Eclipse IDE for Enterprise Java Developers 2019-12 (4.14.0)<br>

**설치**<br>
1. JDK [설치](https://www.oracle.com/technetwork/java/javase/downloads/index.html)

2. 톰켓 [설치](http://tomcat.apache.org/)

3. Eclipse [설치](https://www.eclipse.org/downloads/packages/)
    - Help > Eclipse MarketPlace 메뉴 선택
    - sts 검색
    - Spring Tools 3 Add-On 설치
    - Restart
    - File > New > Start Spring Project 프로젝트 생성
    - Run As > Spring Boot App 프로젝트 제대로 생성 되었는지 확인

4. Mysql [설치](https://dev.mysql.com/downloads/mysql)

    터미널로 비밀번호를 설정한다.
    ```
    cd /usr/local/mysql/bin
    ./mysql -uroot -p
    ```

5. Mysql Workbench [설치](https://dev.mysql.com/downloads/workbench)
    
    새로운 커넥션 생성, password는 앞에서 설정한 걸로 연결한다.

6. Postman [설치](https://www.postman.com/downloads/)
<br>

**프로젝트 셋팅**<br>
1. File > New > Start Spring Project 프로젝트 생성
(프로젝트 생성 후 구성, 패키지, 디렉토리, 파일 등의 경로와 이름 등 참고)

2. pom.xml 수정
    - DB 접속 관련 dependency 추가
    ```
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-jdbc</artifactId>
		</dependency>
		
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<scope>runtime</scope>
		</dependency>
		
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis-spring</artifactId>
			<version>1.3.0</version>
		</dependency>

		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>3.4.1</version>
		</dependency>
    ```
    - web application 관련 dependency 추가  
    ```
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
    ```
3. application.properties 수정
    	```
    	#port
    	server.port = 8090 

    	spring.datasource.driverClassName=com.mysql.cj.jdbc.Driver
    	spring.datasource.url=jdbc:mysql://localhost/sys?characterEncoding=UTF-8&serverTimezone=UTC
    	spring.datasource.username=root
    	spring.datasource.password=11111111
    	```
    
    ***이슈**<br>
    <code>The server time zone value ‘KST’ is unrecognized or represents more than one time zone : mysql-connector-java</code><br>
    해결! spring.datasource.url 뒤에 ?characterEncoding=UTF-8&serverTimezone=UTC 추가, mysql에서 해도 된다.<br>
   
    ***이슈**<br>
    <code>Web server failed to start. Port 8080 was already in use.</code><br>
    해결! 포트를 바꿔주거나 죽이면된다.<br> 
    - 포트 지정 ```server.port = (포트번호)```<br>
    - 터미널에서 특정 포트 kill 명령 ```lsof -i :(포트번호) ```, ```kill -9 (PID)```
   
4. mybatis-config.xml 생성
	```
	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE configuration PUBLIC "-//www.mybatis.org//DTD Config 3.0//EN" "http://www.mybatis.org/dtd/mybatis-3-config.dtd"> 
     
	<configuration>
     
	</configuration>
	```
5. 개발 시작! 하기 전에 CRUD 테스트기능만 넣어보았다. [download here]()
