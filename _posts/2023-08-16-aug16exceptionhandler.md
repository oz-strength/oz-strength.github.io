---
layout: single
title: "예외처리"
categories: spring
tag: [spring, exceptionHandler]
toc: true
author_profile: false
sidebar:
  nav: "docs"
typora-root-url: ../
---



# 예외처리 

1. @ExceptionHandler와 @ControllerAdvice

예외처리를 위한 메서드를 작성하고 @ExceptionHandler를 붙인다. 

```java

@Controller
public class ExceptionController {
	
	@ExceptionHandler({NullPointerException.class, FileNotFoundException.class})
	public String catcher2(Exception ex, Model m) {
		m.addAttribute("ex", ex);
		return "error";
	}
	
	@ExceptionHandler(Exception.class)
	public String catcher(Exception ex, Model m ) {
		System.out.println("catcher() in ExceptionController");
		System.out.println("m="+m);
		m.addAttribute("ex", ex);
		return "error";
	}
	
	@RequestMapping("/ex")
	public String main(Model m) throws Exception{
		m.addAttribute("msg", "message from ExceptionController.main()");
			throw new Exception("예외가 발생했습니다!!");
	}
	
	@RequestMapping("/ex2")
	public String main2() throws Exception{
			throw new FileNotFoundException("예외가 발생했습니다!!");
	}
}
```

