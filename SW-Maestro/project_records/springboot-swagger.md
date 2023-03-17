# Swagger 연결


<div class="aside">
<div class="title">
📌 [개발환경]
</div>
<div class="contents">
- spring boot version: 2.7.2 <br/>
- gradle <br/>
- IntelliJ <br/>
- jdk 17 <br/>
</div>
</div>

이제 어느 정도 기능이 완성되어서 Swagger를 추가해 배포후 Swagger를 api docs로 사용하고자 했다. 

[https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui](https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui) 에서 사람들이 가장 많이 사용하는 2.9.2버전과 검색 결과 가장 적용방법으로 가장 많이 나왔던 방법대로 `springfox-swagger-ui` 과 `springfox-swagger2` 을 추가해 실행해 “http://localhost:8080/swagger-ui.html” “****Unable to infer base url****”과 같은 에러가 발생했다.

이에 대해 검색해보니 [SecurityConfig를 overide하는 것](https://andalmog.tistory.com/9)부터 여러 방식의 해결 방법들을 적용해보았지만 해결이 되지 않았다.

- WebSecurityConfigurerAdapter를 상속받아 Security를 설정하는 방식이 `'org.springframework.security:spring-security-web:5.7.x'` 부터는 적용되지 않는다고 한다. @Bean을 사용하래서 [이 글](https://ssdragon.tistory.com/108)을 참고해 설정했었다.

계속 삽질하다가 [https://velog.io/@dlalscjf94/swagger](https://velog.io/@dlalscjf94/swagger) 를 보고 처음부터 다시 따라했더니 Swagger 적용에 성공할 수 있었다.

## `build.gradle` 에 의존성 추가

```
dependencies {
	implementation(group: 'io.springfox', name: 'springfox-boot-starter', version: '3.0.0')
}
```

`springfox-boot-starter` 를 이용하면 한 번에 swagger 이용에 필요한 dependency들이 추가된다고 한다.

swagger 3.0 버전부터는 “http://localhost:8080/swagger-ui/index.html”에 접속해야 docs를 확인할 수 있다.

## SwaggerConfig 설정

```java
@Configuration
public class SwaggerConfig {

    @Bean
    public Docket api() {
        return new Docket(DocumentationType.OAS_30)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.any())
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("triget Spring Boot REST API")
                .version("1.0.0")
                .description("triget REST API")
                .build();
    }
}
```

처음엔 다른 글을 보고 `Docket(DocumentationType.Swaager_2)` 을 사용했는데 계속 404 error와 함께 Whitelabel 페이지가 나왔다.

검색을 해봤을 때 `@EnableWebMvc`를 사용하라는 등 여러 해결 방법이 많이 나왔으나 나는 `Docket(DocumentationType.OAS_30)` 로 코드를 변경하였더니 해결할 수 있었다.

*단, 현재 내가 설정한 `apiInfo()` 의 내용이 제대로 설정되지 않는 것 같다…*

## path match 설정

<blockquote data-ke-style="style2" style="color: red;">
Caused by: java.lang.NullPointerException: Cannot invoke "org.springframework.web.servlet.mvc.condition.PatternsRequestCondition.getPatterns()" because "this.condition" is null
</blockquote>

swagger를 설정하는 과정에서 위와 같은 에러가 떴다.

찾아보니 SpringBoot 2.6 이후부터 spring.mvc.pathmatch.matching-strategy가 기존의 “ant-path-matcher”방식에서 “path-pattern-parser”로 바뀌었기 때문이라고 한다.

따라서 `application.yml` 에 spring.mvc.pathmatch.matching-strategy를 ant-path-matcher로 바꿔주는 설정을 추가했다.

```yaml
spring:
  mvc:
    pathmatch:
      matching-strategy: ant-path-matcher
```

## 추가

검색해보다가 [springdoc을 쓰라는 글](https://ssdragon.tistory.com/108)을 봤는데 일단 Swagger 적용에 성공했으니 나중에 공부해보도록 하자…