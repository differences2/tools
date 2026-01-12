密码加密方法-SpringBoot

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Beanß
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```