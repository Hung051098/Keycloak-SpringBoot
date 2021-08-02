Dựng Keycloak trong Spring Boot
B1: Tải Keycloak: Keycloak - Downloads Archive - 12.0.4
• Giải nén rồi bật cmd gõ: ./standalone.sh -Djboss.socket.binding.port-offset=100

B2: Truy cập Welcome to Keycloak
• Tạo account 












•  Đăng nhập


• Tạo Realm
 

• Add Client
 

Root URL: là địa chỉ và port start project

• Add Roles

  
• Add U	ser



• Set lại password


•  Add role cho user


B3: Quay lại Realm setting 
• Click vào OpenID Endpoint Configuration


 
• Tiếp đó Gọi API 


•  Tiếp đó vào phần Client click vào sso_login nãy mới tạo

Copy secret_key
 
• Gọi Postman


 grant_type: để mặc đinh là password
⇨ Đã lấy được access_token
B4: Tạo project Spring Boot
•  Tạo Spring Boot Project



• Add  Dependencies


• File pom như sau:
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

• Đổi version Spring boot

•  Cấu hình .properties
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

•  Để phân quyền cho acc có role là user và url là /rest/* cấu hình .properties như sau:
       keycloak.security-constraints[0].authRoles[0]=user
keycloak.security-constraints[0].securityCollections[0].patterns[0]=/rest/*
•  Rest Api
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

•  Gọi api


•  Config Spring security
Thêm Dependencies
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-security</artifactId>
			<version>2.4.3</version>
	</dependency>

• SecurityConfig Class
 
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
•  Comment lại phân quyền trong file .properties
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

