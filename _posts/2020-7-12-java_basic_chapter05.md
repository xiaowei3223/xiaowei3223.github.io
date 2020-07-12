---
layout: post
title: "java基础案例教程--第五章： Java API"
date: 2020-7-12 
description: "Java"
tags: "java基础案例教程"
---

**API （Application Programming Interface）应用程序编程接口**

# 5.1 String类和StringByte类

##	5.1.1 String类概述
	
		在操作String类之前，首先需要对String类进行初始化，在Java中可以通过以下两种方式对String类进行初始化。
			1. 使用字符串常量直接初始化一个String对象：如	
					String str1 = "abc";
			2.使用String构造方法初始化字符串对象，String构造方法如下：
				String()				创建一个内容为空的字符串
				String(String value)	根据指定的自负床内容创建对象
				String(char[] value)	根据指定的字符数组创建对象
				
##	5.1.2 String类的常见操作
	
		1.字符串的基本操作
			length() 字符串长度
			charAt()  某个位置上的字符是什么
			indexof() 子字符串第一次出现的位置
			lastIndexOf() 子字符串最后一次出现的位置
		2. 字符串的转换操作
			toCharArray(); //将字符串转化为字符串组
			valueOf(int)   //将int类型的证书转为字符串
			toUpperCase()	//将字符串中的字母都转换成大写。
		3.字符串的替换和去除空格操作
			replace(oldchar, newchar) 替换
			trim()	去除字符串两边的空格
			replace(" ", "") 去除字符串所有空格
		4.字符串的判断操作
			判断是否以字符串Str开头：startsWith("Str") );
			判断是否已字符串ng结尾： endsWith("ng") );
			判断是否包含字符串tri：  contains("tri") );
			判断字符串是否为空： 	 isEmpty() );
			判断两个字符串是否相等： equals(s2));
		注：“==”和equals()两种方式对字符串进行比较：
			>>>equals()用于比较两个字符串中的字符是否相等
			>>>"=="用于比较两个字符串对象的地址是否相等
		5.字符串的截取和分割
			从第5个字符截取到末尾的结果： 			substring(4));
			从第5个字符截取到第6个字符的结果：		substring(4,6));
			以"-"分割字符串							str.split("-")
			
##	5.1.3 StringBuffer类
	
		StringBuffer类，也称字符串缓冲区。与String类不同的是：它的内容和长度都是可以改变的。
		StringBuffer类似一个字符容器，当在其中添加或删除字符时，并不会产生新的StringBuffer对象。
		字符串操作：
			添加：
				append(chr) 				//在末尾添加字符串
				insert(num1, chr) 			//在指定位置插入字符串
			删除		
				delete(num1,num2) 			//指定范围删除
				deleteCharAt(num1); 		//指定位置删除
				delete(0, sb.length());		//清空缓冲区结果
			修改
				setCharAt(num1, chr); 		//修改指定位置字符
				replace(num1, num2, chr); 	//替换指定位置字符
				sb.reverse() 				//字符串翻转 
			如：
				public static void add() {
					StringBuffer sb = new StringBuffer(); //定义一个字符串缓冲区
					sb.append("abcdefg"); //在末尾添加字符串
					System.out.println("append添加结果： " + sb);
					sb.insert(2, "123"); //在指定位置插入字符串
					System.out.println("insert添加结果： " + sb);
					
				}
				public static void remove() {
					StringBuffer sb = new StringBuffer("abcdefg");
					sb.delete(1, 5); //指定范围删除
					System.out.println("删除指定范围结果： " + sb);
					sb.deleteCharAt(2); //指定位置删除
					System.out.println("删除指定位置结果：  " + sb);
					sb.delete(0, sb.length());//清空缓冲区结果
					System.out.println("清空缓冲区结果： " + sb);
				}
				public static void alter() {
					StringBuffer sb = new StringBuffer("abcdefg");
					sb.setCharAt(1, 'p'); //修改指定位置字符
					System.out.println("修改指定位置字符结果： " + sb);
					sb.replace(1, 3, "qq"); //替换指定位置字符
					System.out.println("替换指定位置字符结果：  " + sb);
					System.out.println("字符串翻转结果： " + sb.reverse());
				}
				
# 5.2 System类与Runtime类

##	5.2.1 System类
		System类定义了一些与系统相关的属性和方法，它所提供的属性和方法都是静态的。
		因此，想要引用这些属性和方法，直接用System类调用即可。
			 System.getProperties(); //获取当前系统属性	
		//获取所有系统属性的key（属性名），返回Set对象
			Set<String> propertyNames = properties.stringPropertyNames()
		//获取当前key（属性名）所对应的值（属性值）
			String value = System.getProperty(key);
		System.currentTimeMillis() 返回一个long类型的值，
			这个值表示当前时间与1970年1月1日0时0分0秒之间的时间差，
			单位时毫秒，通常也将该值称为时间戳。
		将一个数组中的元素快速拷贝到另一个数组：
			System.arraycopy(src, srcPos, dest, destPos, length);
				src: 表示源数组
				dest：表示目标数组
				srcPos: 表示源数组中拷贝元素的起始位置
				destPos：表示拷贝到目标数组的起始位置
				length： 表示拷贝元素的个数
				
##	5.2.2 Runtime类
		Runtime类用于表示虚拟机运行时的状态，用于封装JVM虚拟机进程。
		每次使用java命令启动虚拟机都对应一个Runtime实例，并且只有一个实例，
		因此该类采用单例模式进行设计，对象不可以直接实例化。
		如：
			Runtime rt = Runtime.getRuntime(); //获取
			System.out.println("处理器的个数： " + rt.availableProcessors() + "个");
			System.out.println("空闲内存数量： " + rt.freeMemory()/1024/1024 + "M");
			System.out.println("最大可用内存数： " + rt.maxMemory()/1024/1024 + "M");
		调用Runtime对象的exec方法，并将系统命令notepad.exe作为
		参数传递给方法，运行程序会在桌面上打开一个记事本
			public static void main(String[] args) throws IOException {
				Runtime rt = Runtime.getRuntime(); 
				rt.exec("notepad.exe");
			}
		关闭程序使用destroy()方法
			public static void main(String[] args) throws IOException, InterruptedException {
				Runtime rt = Runtime.getRuntime(); 
				Process process = rt.exec("notepad.exe"); //得到表示进程的Process对象
				Thread.sleep(3000);	//程序休眠3秒
				process.destroy(); 	//杀掉程序
			}
			
# 5.3 Math类与Random类

##	5.3.1 Math类
		Math类是数学操作类，提供了一系列用于数学运算的静态方法。包括求绝对值、三角函数等。
		Math类中有两个静态常量PI和E，分别代表数学常量Π和e。
		计算绝对值的结果： Math.abs(-10));
		求大于参数的最小整数： Math.ceil(5.6));
		求小于参数的最大整数： Math.floor(-4.2));
		对小数进行四舍五入的结果： Math.round(-4.6));
		求两个数的较大值： Math.max(3.7, 6.9));
		求两个数的较小值： Math.min(3.7, 6.9));
		生成一个大于等于0.0小于1.0随机值： Math.random());
		
##	5.3.2 Random类
		Random() 创建一个伪随机数生成器
		Random(long seed) 使用一个long型的seed种子创建伪随机数生成器
		Random类的常用方法：
		boolean nextBoolean()		随机生成boolean类型的随机数
		double nextDouble()			随机生成double类型的随机数
		float nextFloat()			随机生成float类型的随机数
		int nextInt()				随机生成int类型的随机数
		int nextInt(int n)			随机生成0~n的int类型的随机数
		long nextLong()				随机生成long类型的随机数
		
# 5.4 包装类
	包装类将基本数据类型的值包装为引用数据类型的对象。
	基本数据类型： byte、char、int、short、long、float、double、boolean