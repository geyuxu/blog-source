---
title: "Java中Map转换为指定对象bean"
date: 2015-11-07 17:19:39
tags: [java,反射]
---

调用方式：Student stu = BeanUtil.convertMap(Student.class,map);

<!--more-->

	import java.beans.BeanInfo;
	import java.beans.Introspector;
	import java.beans.PropertyDescriptor;
	import java.lang.reflect.Field;
	import java.math.BigDecimal;
	import java.util.Date;
	import java.util.Map;

	public class BeanUtil{
		public static <T> T convertMap(Class<T> clazz, Map map) {
          
            BeanUtil.pretreatment(clazz,map);
            
            T obj=null;
            try {
                obj = clazz.newInstance(); // 创建 JavaBean 对象
                BeanInfo beanInfo = Introspector.getBeanInfo(clazz); // 获取类属性
                // 给 JavaBean 对象的属性赋值
                PropertyDescriptor[] propertyDescriptors = beanInfo.getPropertyDescriptors();
                for (int i = 0; i < propertyDescriptors.length; i++) {
                    PropertyDescriptor descriptor = propertyDescriptors[i];
                    String propertyName = descriptor.getName();
                    if (map.containsKey(propertyName)) {
                        // 下面一句可以 try 起来，这样当一个属性赋值失败的时候就不会影响其他属性赋值。
                        Object value = map.get(propertyName);
                        Object[] args = new Object[1];
                        args[0] = value;
                        descriptor.getWriteMethod().invoke(obj, args);
                    }
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
            return obj;
        }
    
        private static <T> void pretreatment(Class<T> clazz, Map map) {
    
            Field[] declaredFields = clazz.getDeclaredFields();
            for(Field field:declaredFields){
                String fieldName = field.getName();
                Object mo = map.get(fieldName);
                if(mo == null)
                    continue;
                if(field.getType() == Date.class){
                    if(mo instanceof Number){
                        mo = new Date((Long) mo);
                    }
                }else if(Number.class.isAssignableFrom(field.getType())){
                    if(mo instanceof String || mo instanceof Character) {
                        if (field.getType() == BigDecimal.class) {
                            mo = BigDecimal.valueOf(Long.valueOf((String)mo));
                        }else if(field.getType() == Integer.class){
                            mo = Integer.valueOf((String)mo);
                        }else if(field.getType() == Short.class){
                            mo = Short.valueOf((String)mo);
                        }
                    }
                }
                map.put(fieldName,mo);
            }
        }
	}
	
	
方法pretreatment作用是预处理类型转换，保证转换不会出错。