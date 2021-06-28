# framework-demo-cors
## CORS 적용방법
#### 스프링부트에 CORS를 적용할 수 있는 방법은 두 가지 정도 있는 것 같다.

1. @CrossOrigin 추가
```
@CrossOrigin(orgins = "http://localhost:8080") // TODO 허용도메인으로 변경
@RestController
public class ApiController {

    @GetMapping("/api/v1")
    public String hello() {
        return "hello";
    }
}
```
위 코드와 같이 @CrossOrigin 어노테이션을 추가하면 CORS를 적용할 수 있다.

2. WebConfig에 CORS 설정
```
@Configuration
public class WebConfig implements WebMvcConfigurer {

  @Override
  public void addCorsMappings(CorsRegistry registry) {
    registry.addMapping("/**")
        .allowedOrigins("http://localhost:8080"); // TODO 허용도메인으로 변경
  }
}
```
위 코드와 같이 WebMvcConfigurer를 상속 받아 CORS를 적용할 수 있다.
localhost:8080에서 오는 요청은 어느 url이든 CORS가 적용된다.

3. spring security를 사용한 경우 spring security 설정에 cors 설정을 추가할 수 있다.
```
public class SecurityConfig extends WebSecurityConfigurerAdapter {
  @Override
  protected void configure(HttpSecurity http) throws Exception {
    http
      .cors()
      .configurationSource(corsConfigurationSource()) 
      .and()
      ...
  }
		
  @Bean
  public CorsConfigurationSource corsConfigurationSource() {
    CorsConfiguration configuration = new CorsConfiguration();
    configuration.addAllowedOrigin("htto://allowwed.domain.com"); // TODO 허용도메인으로 변경
    configuration.addAllowedHeader("*");
    configuration.addAllowedMethod("*");
    configuration.setAllowCredentials(true);

    UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
    source.registerCorsConfiguration("/**", configuration);
    return source;
  }
  ...
}
```
