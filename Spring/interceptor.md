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