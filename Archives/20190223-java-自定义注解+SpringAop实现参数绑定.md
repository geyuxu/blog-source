---
title: "自定义注解+SpringAop实现参数绑定"
date: 2015-05-23
tags: [java,注解,spring mvc,aop,RequestBody]
---

实现自定义注解@RequestBodyMap，以下是我们的调用demo。

	@RequestMapping(value = "/test.htm", method = RequestMethod.POST)
	@ResponseBody
	public Person test(HttpServletRequest request,@RequestBodyMap Person person) {
		//操作
		return person;
	}
<!--more-->
加了@RequestBodyMap注解的person会自动绑定浏览器传过来的数据，包括json、map、xml等。

@RequestBodyMap.java:

	...
	@Target(ElementType.PARAMETER)
	@Retention(RetentionPolicy.RUNTIME)
	@Documented
	public @interface RequestBodyMap {
	
	}
	...


@Target说明了Annotation所修饰的对象范围：包、类、字段、方法等  
取值有：  
1. ElementType.CONSTRUCTOR:用于描述构造器  
2. ElementType.FIELD:用于描述域  
3. ElementType.LOCAL_VARIABLE:用于描述局部变量  
4. ElementType.METHOD:用于描述方法  
5. ElementType.PACKAGE:用于描述包  
6. ElementType.PARAMETER:用于描述参数  
7. ElementType.TYPE:用于描述类、接口(包括注解类型) 或enum声明  

@Retention定义了该Annotation被保留的时间长短：  
1. RetentionPolicy.SOURCE：仅出现在源代码中，而被编译器丢弃  
2. RetentionPolicy.CLASS：编译在class文件中，运行时不被保留；   
3. RetentionPolicy.RUNTIME：运行时保留；



@Documented用于描述其它类型的annotation应该被作为被标注的程序成员的公共API，因此可以被例如javadoc此类的工具文档化。Documented是一个标记注解，没有成员。

@Inherited阐述了某个被标注的类型是被继承的。如果一个使用了@Inherited修饰的annotation类型被用于一个class，则这个annotation将被用于该class的子类。

ConverterInterceptor.java:
	
	...
	@Aspect
	public class ConverterInterceptor {
		
		/**
		 * 定义一个切入点
		 */
		@Pointcut("(execution(* com.geyuxu.*.*(..)))")  
	    private void anyMethod(){}  
	      
	    /**
	     * 方法描述：  
	     * 注入带RequestBodyMap注解的参数
	     * 1、判断参数中是否有RequestBodyMap注解<br/>
	     * 2、将json或map转换成对象<br/>
	     * 3、执行方法<br/>
	     * @---
title: doBasicProfiling 
	     * @date 2015年5月21日 下午7:17:35
	     * @author 葛于旭
	     * @modifier 
	     * @modifydate 
	     * @param pjp
	     * @return
	     * @throws Throwable
	     */
	    @Around("anyMethod()")  
	    public Object doBasicProfiling(ProceedingJoinPoint pjp) throws Throwable{  
	
			/**
			 * 获取参数注解
			 */
	    	MethodSignature signature = (MethodSignature) pjp.getSignature();
	    	Method method = signature.getMethod();
	    	Annotation[][] annss = method.getParameterAnnotations();
	
			/**
			 * 判断参数中是否有注解
			 */
	    	int idx = 0;
	    	loop:for(Annotation[] anns : annss){
	    		for(Annotation ann : anns){
	    			if(ann instanceof RequestBodyMap){
	    				break loop;
	    			}
	    		} 
	    		idx++;
	    	}
	    	
	    	Object object = null;
	    	
	    	if(idx != annss.length){
	    		Object[] args = pjp.getArgs();
	        	HttpServletRequest request = (HttpServletRequest) args[0];
	        	
	        	@SuppressWarnings("rawtypes")
	    		Class[] clazzs = method.getParameterTypes();
	        	
				//此方法自己实现，根据请求头决定如何转换对象
	        	@SuppressWarnings("unchecked")
				Object param = HttpMessageConverterUtil.getObjectFromInputStream(
	    				request, clazzs[idx]);
	        	//执行方法 
	        	object = pjp.proceed(new Object[]{args[0],args[1],param});
	    	}else{
	    		object = pjp.proceed();
	    	}
	    
	        return object;  
	    }     
	}

applicationContext.xml:
	
	...
	<aop:aspectj-autoproxy/>
	
	<bean class="com.geyuxu.interceptor.ConverterInterceptor" />
	...

