## 인터셉터


## 생각정리
```
인터셉터가 없으면 매번 컨트롤러에서 중복된 코드가 발생한다. 인증관련된 코드들은 인터셉터로 관리가 된다면 모든 요청이 컨트롤러에 전달 되기전에 
쉽게 관리할 수 있다. 또한 중복된 코드가 아니라 하나의 코드로 이러한 과정들을 관리할 수 있다.
AOP관점에서 좋은 전략이므로 이해하고 사용하도록 해보자.

```


## 스프링부트 doc
```
In order to understand how a Spring interceptor works, let's take a step back and look at the HandlerMapping.

The purpose of HandlerMapping is to map a handler method to a URL. That way, the DispatcherServlet will be able to invoke it when processing a request.

As a matter of fact, the DispatcherServlet uses the HandlerAdapter to actually invoke the method.

In short, interceptors intercept requests and process them. They help to avoid repetitive handler code such as logging and authorization checks.

```



## intercepter 적용

- 헤더값에  인증 토큰이 있는지 확인할 수 있다. 
- 이렇게 앞에서 처리를하면 중복 코드를 줄일 수 있다.

```java

// WebConfig
@Configuration
@RequiredArgsConstructor
public class WebConfig implements WebMvcConfigurer {
    private final AuthenticationInterceptor authenticationInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(authenticationInterceptor)
                .order(1)
                .addPathPatterns("적용할 url")
                .excludePathPatterns("제외할 url");
    }

}
// intercepter


@Slf4j
@Component
@RequiredArgsConstructor
public class AuthenticationInterceptor implements HandlerInterceptor {
    
    @Override
    public boolean preHandle(HttpServletRequest request,
                             HttpServletResponse response, Object handler) throws Exception {

        //조건설정하기
        //인증헤더가 없을 경우 리턴  
        String authorization = request.getHeader("auth");
        if (!StringUtils.hasText(authorization)) {
            throw new AuthException(ErrorCode.NOT_EXISTS_AUTHORIZATION);
        }
        return true;
    }
}


```