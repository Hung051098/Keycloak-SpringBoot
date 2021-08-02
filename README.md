Dựng Keycloak trong Spring Boot
B1: Tải Keycloak: Keycloak - Downloads Archive - 12.0.4
•	Giải nén rồi bật cmd gõ: ./standalone.sh -Djboss.socket.binding.port-offset=100

 ![image](https://user-images.githubusercontent.com/39266629/127840380-d78122ff-9000-4e13-8a77-da894d334b4e.png)

B2: Truy cập Welcome to Keycloak
•	Tạo account 
 

![image](https://user-images.githubusercontent.com/39266629/127840390-c1ff0474-38ed-409d-b09a-b04c1f5415ef.png)










•	 Đăng nhập
 
![image](https://user-images.githubusercontent.com/39266629/127840416-afd93abf-36c3-4269-be1c-ba4ee5a895c6.png)

•	Tạo Realm

![image](https://user-images.githubusercontent.com/39266629/127840437-feff51c9-dd2a-4bf3-ab4d-ee8033eb3a0c.png)
![image](https://user-images.githubusercontent.com/39266629/127840453-36e096f2-985c-4b58-acc3-4363dc26738d.png)


•	Add Client

![image](https://user-images.githubusercontent.com/39266629/127840462-aed8cca2-0aec-44f4-83a8-7f1054b4b931.png)
![image](https://user-images.githubusercontent.com/39266629/127840475-428d02f5-95c2-4553-9e9f-c38c3636ce25.png)


Root URL: là địa chỉ và port start project

•	Add Roles

![image](https://user-images.githubusercontent.com/39266629/127840497-8432414e-98e8-4b76-83de-a0a9f06eb826.png)
![image](https://user-images.githubusercontent.com/39266629/127840521-4af4651c-f9d7-4443-ae31-4b8d6d971703.png)

    
•	Add User

 ![image](https://user-images.githubusercontent.com/39266629/127840538-ad76c789-7fcb-4631-a3bf-b8d0eb0b2891.png)
![image](https://user-images.githubusercontent.com/39266629/127840550-16daee63-9eb4-4dc2-b5f8-b54b00379cda.png)

 

•	Set lại password
![image](https://user-images.githubusercontent.com/39266629/127840569-8127acd2-d3ef-43c4-8355-637657c2c8f2.png)
![image](https://user-images.githubusercontent.com/39266629/127840582-222d07c3-3638-4ca6-be30-3507d9ab5557.png)

 
•	 Add role cho user

 ![image](https://user-images.githubusercontent.com/39266629/127840596-ec4aaa34-a339-409c-8500-0447c4175e71.png)


B3: Quay lại Realm setting 
•	Click vào OpenID Endpoint Configuration

![image](https://user-images.githubusercontent.com/39266629/127840619-06e7930c-0ca4-4ec9-8f83-33db41061bb3.png)

 
 
•	Tiếp đó Gọi API 

 ![image](https://user-images.githubusercontent.com/39266629/127840639-359bbfc6-de74-4487-bf04-90c3322bed39.png)


•	 Tiếp đó vào phần Client click vào sso_login nãy mới tạo

 ![image](https://user-images.githubusercontent.com/39266629/127840654-5471f13e-8ea8-42be-8e42-49bcf9a90dde.png)

Copy secret_key
 
•	Gọi Postman

 ![image](https://user-images.githubusercontent.com/39266629/127840683-50629d87-815b-4981-8f6e-1c672a3f0b71.png)


 grant_type: để mặc đinh là password
	Đã lấy được access_token
B4: Tạo project Spring Boot
•	 Tạo Spring Boot Project

 ![image](https://user-images.githubusercontent.com/39266629/127840761-c14e2962-45ce-4add-b29a-20fdbff9018c.png)
![image](https://user-images.githubusercontent.com/39266629/127840786-398040c0-9b6e-47bf-a784-43984d9fa54a.png)

 

•	Add  Dependencies

 ![image](https://user-images.githubusercontent.com/39266629/127840800-7aaebfcd-e4b8-41ce-96ac-b0b8c6ffc529.png)
![image](https://user-images.githubusercontent.com/39266629/127840809-90fa4e72-e2f0-49e8-8399-6e1ea2e2daa6.png)

 
•	File pom như sau:
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.0.0.RELEASE</version>
		<relativePath /> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.example</groupId>
	<artifactId>keycloakspringboot</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>keycloakspringboot</name>
	<description>Demo project for Spring Boot</description>
	<properties>
		<java.version>1.8</java.version>
		<keycloak-adapter-bom.version>12.0.4</keycloak-adapter-bom.version>
	</properties>
	<dependencyManagement>
		<dependencies>
			<dependency>
				<groupId>org.keycloak.bom</groupId>
				<artifactId>keycloak-adapter-bom</artifactId>
				<version>${keycloak-adapter-bom.version}</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
		</dependencies>
	</dependencyManagement>
	<dependencies>
	
		<dependency>
			<groupId>org.keycloak</groupId>
			<artifactId>keycloak-spring-boot-starter</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-thymeleaf</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>runtime</scope>
			<optional>true</optional>
		</dependency>
		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
			<optional>true</optional>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<configuration>
					<excludes>
						<exclude>
							<groupId>org.projectlombok</groupId>
							<artifactId>lombok</artifactId>
						</exclude>
					</excludes>
				</configuration>
			</plugin>
		</plugins>
	</build>

</project>

•	Đổi version Spring boot

 ![image](https://user-images.githubusercontent.com/39266629/127840834-4350c5b4-63e9-468c-837c-86a86bfbbf21.png)

•	 Cấu hình .properties
### server port
server.port=22001

#Keycloak Configuration
keycloak.auth-server-url=http://localhost:8180/auth
keycloak.realm=keycloakspringboot
keycloak.resource=sso_login
keycloak.public-client=true
#keycloak.security-constraints[0].authRoles[0]=user
#keycloak.security-constraints[0].securityCollections[0].patterns[0]=/customers/*
keycloak.principal-attribute=preferred_username
keycloak.credentials.secret =452b4a3f-d176-4c0c-aae3-fa73b28f3ccf

•	 Để phân quyền cho acc có role là user và url là /rest/* cấu hình .properties như sau:
keycloak.security-constraints[0].authRoles[0]=user
keycloak.security-constraints[0].securityCollections[0].patterns[0]=/rest/*
•	 Rest Api
package com.example.keycloakspringboot.rest;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/rest")
public class ApiRest {

	@GetMapping("/hung")
	public String work() {
		return "Working ...";
	}
}

•	 Gọi api

 ![image](https://user-images.githubusercontent.com/39266629/127840858-f6730e63-305e-419a-bc8c-afc8b144666f.png)
![image](https://user-images.githubusercontent.com/39266629/127840867-42bce3cd-e638-4721-b240-3465fe7374c0.png)

 
•	 Config Spring security
Thêm Dependencies
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-security</artifactId>
			<version>2.4.3</version>
	</dependency>

•	SecurityConfig Class
 
@Configuration
@EnableWebSecurity
@ComponentScan(basePackageClasses = KeycloakSecurityComponents.class)
public class SecurityConfig extends KeycloakWebSecurityConfigurerAdapter {
@Autowired
    public void configureGlobal(
      AuthenticationManagerBuilder auth) throws Exception {
 
        KeycloakAuthenticationProvider keycloakAuthenticationProvider
          = keycloakAuthenticationProvider();
        keycloakAuthenticationProvider.setGrantedAuthoritiesMapper(
          new SimpleAuthorityMapper());
        auth.authenticationProvider(keycloakAuthenticationProvider);
    }

    @Bean
    public KeycloakSpringBootConfigResolver KeycloakConfigResolver() {
        return new KeycloakSpringBootConfigResolver();
    }

    @Bean
    @Override
    protected SessionAuthenticationStrategy sessionAuthenticationStrategy() {
        return new RegisterSessionAuthenticationStrategy(
          new SessionRegistryImpl());
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        super.configure(http);
        http.authorizeRequests()
          .antMatchers("/rest/*")
          .hasRole("user")
          .anyRequest()
          .permitAll();
    }
}
•	 Comment lại phân quyền trong file .properties
### server port
server.port=22001

#Keycloak Configuration
keycloak.auth-server-url=http://localhost:8180/auth
keycloak.realm=keycloakspringboot
keycloak.resource=sso_login
keycloak.public-client=true
keycloak.principal-attribute=preferred_username
keycloak.credentials.secret =452b4a3f-d176-4c0c-aae3-fa73b28f3ccf

#keycloak.security-constraints[0].authRoles[0]=user
#keycloak.security-constraints[0].securityCollections[0].patterns[0]=/rest/*
B5: Gọi lại API
Tài liệu tham khảo: A Quick Guide to Using Keycloak with Spring Boot | Baeldung
Kết thúc
