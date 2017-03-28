---
title: SpringMVC学习笔记
date: 2017-02-19 22:56:00
tags: SpringMVC
categories: Java EE
---

## SpringMVC框架

![](https://ooo.0o0.ooo/2017/03/26/58d74e9297b73.png)

SpringMVC是Spring的一个基于MVC的Web的解决方案。具体的架构图如下:

![58d75045ad2a8.png](https://ooo.0o0.ooo/2017/03/26/58d75045ad2a8.png)

配置需要:

![58d75e2e64a24.png](https://ooo.0o0.ooo/2017/03/26/58d75e2e64a24.png)

DispatcherServlet前端控制器加载DispatcherServlet.properties文件，从而加载各个组件。如果我们的配置了这些处理映射器，处理器映射器，等等，那么以我们的为准。



组件扫描可以扫@controller, @service,@component,@responsity



无法直接从string类型绑定日期，需要自定义日期类型的绑定，方法:

- 第一个方法：在controller下自定义属性编辑器@initbinder
- 第二种方式: 自定义参数绑定，使用转换器

## 数据回显

使用`@ModelAttribute`

## 集合类型参数绑定

### 绑定数组

```java
    /**
     * 删除商品
     * @param delete_id
     * @return
     * @throws Exception
     */
    @RequestMapping("/deleteItems")
    public String deleteItems(Integer[] delete_id) throws Exception {
        return "success";
    }
```

```xml
<td><label>
       <input type="checkbox" name="delete_id" value="${item.id}">
                </label></td>
```



### 绑定list

```html
<td><input value="${item.name}" name="itemsCustoms[${s.index}].name" type="text"/></td>
                <td><input value="${item.price}" name="itemsCustoms[${s.index}].price" type="text"/></td>
```



### 绑定map

```java
itemsCustoms['key1']
  itemsCustoms['key2']
```



## 图片上传

不要把图片上传到工程目录



## json数据的交互

- 使用@RequestBody将json转换为pojo。
- 使用@ResponseBody可以将java对象转成json输出

## validation校验器

JSR-303校验规范。（基于JavaEE6）spring使用的是Hibernate-validation校验器

## 统一异常处理器

非常重要

1. 预期的异常，及时对判断做的好。针对这种问题，手动的处理，可以捕获，也可以向上抛出。
2. 针对运行时异常，我们通过规范代码质量和系统测试时，详细测试来排除运行时的异常。

### 定义异常

```java
package tech.jiangtao.springmvc.exception;

/**
 * @class: CustomException </br>
 * @description: 自定义异常 </br>
 * @creator: kevin </br>
 * @email: jiangtao103cp@gmail.com </br>
 * @date: 2017/3/28 下午8:57</br>
 * @version: 0.0.1 </br>
 **/
public class CustomException extends Exception {

  private String message;

  public CustomException(String message){
    super(message);
    this.message = message;
  }

  @Override public String getMessage() {
    return message;
  }

  public void setMessage(String message) {
    this.message = message;
  }
}

```

```java
package tech.jiangtao.springmvc.exception;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.springframework.web.servlet.HandlerExceptionResolver;
import org.springframework.web.servlet.ModelAndView;

/**
 * @class: CustomExceptionHandler </br>
 * @description: 自定义异常处理器 </br>
 * @creator: kevin </br>
 * @email: jiangtao103cp@gmail.com </br>
 * @date: 2017/3/28 下午9:03</br>
 * @version: 0.0.1 </br>
 * 这是什么jb处理器
 **/
public class CustomExceptionHandler implements HandlerExceptionResolver {

  // 如果是移动端应该怎么处理
  // handler最终是一个HandlerMethod
  @Override public ModelAndView resolveException(HttpServletRequest request,
      HttpServletResponse response, Object handler, Exception e) {
    // 进行输出
    e.printStackTrace();
    // 统一的异常处理代码,根据不同的异常类型进行异常处理
    //  系统自定义的异常CustomException,手动抛出，手动处理
    // 非自定义的异常,我们可以重新构造成自定义异常CustomException.
    String message = null;
    CustomException exception = null;
    if (e instanceof CustomException){
      exception = (CustomException) e;
    }else {
      exception = new CustomException(e.getMessage());
    }
    message = exception.getMessage();
    request.setAttribute("message",message);
    try {
      request.getRequestDispatcher("/WEB-INF/jsp/error.jsp").forward(request,response);
    } catch (ServletException | IOException e1) {
      e1.printStackTrace();
    }
    return null;
  }
}

```

```java
<!--统一的异常处理器-->
  <bean class="tech.jiangtao.springmvc.exception.CustomExceptionHandler"/>
```



## Restful支持

### 配置web.xml

```xml
<!--配置Restful-->
    <servlet>
        <servlet-name>dispatcher_rest</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>/WEB-INF/application-mvc.xml</param-value>
        </init-param>
    </servlet>
    <servlet-mapping>
        <servlet-name>dispatcher_rest</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
```

### 编辑Controller

```java
  @RequestMapping("/viewItems/{id}")
  public @ResponseBody ItemsCustom viewItems(@PathVariable("id") Integer id) throws Exception{
    return itemsService.findItemsById(id);
  }
```



## 拦截器

1. 用于权限控制


### 拦截器的使用场合

可以做用户认证和用户权限拦截



### 测试拦截器

都放行

![58da7e6a58455.png](https://ooo.0o0.ooo/2017/03/28/58da7e6a58455.png)

放一个

![58da7ed6e1aa7.png](https://ooo.0o0.ooo/2017/03/28/58da7ed6e1aa7.png)

都不放

只有前边拦截器preHandle执行.

日志拦截器或者异常拦截器，必须放在拦截器链的第一个，并且预处理方法必须放行

### 拦截器应用（用户认证）

```java
@Controller
public class LoginController {
	
	
	//用户登陆提交方法
	@RequestMapping("/login")
	public String login(HttpSession session, String usercode,String password)throws Exception{
		
		//调用service校验用户账号和密码的正确性
		//..
		
		//如果service校验通过，将用户身份记录到session
		session.setAttribute("usercode", usercode);
		//重定向到商品查询页面
		return "redirect:/items/queryItems.action";
	}
	
	//用户退出
	@RequestMapping("/logout")
	public String logout(HttpSession session)throws Exception{
		
		//session失效
		session.invalidate();
		//重定向到商品查询页面
		return "redirect:/items/queryItems.action";
		
	}
	

}
```

```java
public class LoginInterceptor implements HandlerInterceptor {

	//在执行handler之前来执行的
	//用于用户认证校验、用户权限校验
	@Override
	public boolean preHandle(HttpServletRequest request,
			HttpServletResponse response, Object handler) throws Exception {
		
		//得到请求的url
		String url = request.getRequestURI();
		
		//判断是否是公开 地址
		//实际开发中需要公开 地址配置在配置文件中
		//...
		if(url.indexOf("login.action")>=0){
			//如果是公开 地址则放行
			return true;
		}
		
		//判断用户身份在session中是否存在
		HttpSession session = request.getSession();
		String usercode = (String) session.getAttribute("usercode");
		//如果用户身份在session中存在放行
		if(usercode!=null){
			return true;
		}
		//执行到这里拦截，跳转到登陆页面，用户进行身份认证
		request.getRequestDispatcher("/WEB-INF/jsp/login.jsp").forward(request, response);
		
		//如果返回false表示拦截不继续执行handler，如果返回true表示放行
		return false;
	}
	//在执行handler返回modelAndView之前来执行
	//如果需要向页面提供一些公用 的数据或配置一些视图信息，使用此方法实现 从modelAndView入手
	@Override
	public void postHandle(HttpServletRequest request,
			HttpServletResponse response, Object handler,
			ModelAndView modelAndView) throws Exception {
		System.out.println("HandlerInterceptor1...postHandle");
		
	}
	//执行handler之后执行此方法
	//作系统 统一异常处理，进行方法执行性能监控，在preHandle中设置一个时间点，在afterCompletion设置一个时间，两个时间点的差就是执行时长
	//实现 系统 统一日志记录
	@Override
	public void afterCompletion(HttpServletRequest request,
			HttpServletResponse response, Object handler, Exception ex)
			throws Exception {
		System.out.println("HandlerInterceptor1...afterCompletion");
	}

}
```



