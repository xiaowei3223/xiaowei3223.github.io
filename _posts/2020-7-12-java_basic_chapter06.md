---
layout: post
title: "java基础案例教程--第六章: 集合类"
date: 2020-7-12 
description: "Java"
tags: "java基础案例教程"
---


# 6.1 集合概述
	JDK中提供了一系列特殊的类，这些类可以存储任意类型的对象，
	并且长度可变，在JAVA中这些类被统称为集合。
	集合按照存储结构可以分为两大类：
		单列集合Collection：
			单列集合类的根接口，用于存储一系列符合某种规则的元素，他有两个重要的子接口，
			分别为List和Set。
				List特点： 	元素有序，可重复。主要实现类有ArrayList和LinkedList
				Set特点： 	元素无需，而且不可重复。主要是实现类有HashMap和TreeMap
		双集合Map：
			双列集合类的根接口，用于存储具有键（key)、值（value）映射关系的元素。
			每个元素都包含一对键值，在使用Map集合时，可以通过指定的key找到对应的value。
			Map接口的主要实现类有HashMap和TreeMap.

# 6.2 Collection接口

	Collection接口的方法：
		boolean add(Object o)				向集合中添加一个元素
		boolean addAll(Collection c)		将指定Collection中的所有元素添加到该集合中
		void clear()						删除该集合中的所有元素
		boolean remove(Object o)			删除该集合中的指定元素
		boolean removeAll(Collection c)		删除指定集合中的所有元素
		boolean isEmpty() 					判断该集合是否为空
		boolean contains(Object o)			判断该集合中是否包含某个元素
		boolean containsAll(Collection c)	判断该集合中是否包含指定集合中的所有元素
		Iterator iterator()					返回该集合的元素上进行迭代的迭代器（iterator),用于遍历该集合所有元素
		int size()							获取该集合元素个数

# 6.3 List接口

##	6.3.1 List接口简介
		实现List接口的对象称为List集合。 在List集合中允许出现重复的元素，所有的元素是以一种线性方式
		进行存储的，在程序中可以通过索引来访问集合中的指定元素。
		List集合常用方法：
		void add(int index, Object element)		将元素element插入在List集合的index处
		boolean addAll(int index, Collection c)	将集合c所包含的所有元素插入到List集合的index处
		Object get(int index)					返回集合索引index处的元素
		Object remove(int index)				删除index索引处的元素
		Object set(int index, Object element)	将索引index处元素替换成element对象，并将替换后的元素返回
		int indexOf(Object o)					返回对象o在List集合中出现的位置索引
		int lastIndexOf(Object o)				返回对象o在List集合中最后一次出现的位置索引
		List subList(int fromIndex, int toIndex) 返回从索引fromIndex（包括）到toIndex（不包括）处所有元素集合组成的子集合

##	6.3.2 ArrayList集合
		ArrayList是List接口的一个实现类，是程序中最常见的一种集合。
		可以将ArrayList集合看作一个长度可变的数组。
		ArrayList集合中大部分方法都是从父类Collection和List继承过来的。
		其中add()和get()方法用于实现元素的存取。
		
##	6.3.3 LinkedList集合
		LinkedList维护以个双向循环链表，链表中的每一个元素都使用引用的方式来记住它的前一个元素和后一个元素，
		从而可以将所有的元素彼此连接起来。
		add()		在此列表中指定的位置插入指定的元素
		addFirst()	将指定元素插入此列表的开头
		addLast()	将指定元素添加到此列表的结尾
		getFirst()	返回此列表的第一个元素
		getLast()	返回此列表的最后一个元素
		remove()	删除指定位置的元素
		removeFirst()	删除第一个元素
		removeLast()	删除最后一个元素
		
##	6.3.4 Iterator接口
		Iterator接口主要用于迭代访问（遍历）Collection中的元素，因此Iterator对象也被称为迭代器。
		> 使用hasNext()方法判断集合中是否存在下一个元素
		
##	6.3.5 foreach循环
		格式：
			for (容器中元素类型 临时变量: 容器变量）{执行语句}
		注意：
			当使用foreach循环遍历集合和数组时，只能访问集合中的元素，不能对其中的元素进行修改。
			
# 6.4 Set接口

##	6.4.1 Set接口简介
		Set接口和List接口一样，同样继承自Collection接口。
		Set接口中元素无序，并且都会以某种规则保证存入的元素不出现重复。
		Set接口主要有两个实现类：HashSet和TreeSet
			HashSet根据对象的哈希值来确定元素在集合中的存储位置，因此具有良好的存取和查找性能。
			TreeSet以二叉树的方式来存储元素，可以实现对集合中的元素进行排序。
			
##	6.4.2 HashSet接口
		HashSet所存储的元素是不可重复的，并且元素都是无序的。
		如何向hashSet添加一个对象：
			>>>1. 调用该对象的hashCode()来计算对象的哈希值，从而确定元素的存储位置。
			>>>2. 如果此时哈希值相同，再调用对象的equals()来确保该位置没有重复元素。
			
# 6.5 Map接口

##	6.5.1 Map接口简介
		Map接口是一种双列集合，它的每个元素都包含一个键对象key和值对象value。key和value之间是一中对应关系，称为映射。
		void put(Object key, Object value)	将指定的值与此映射中的指定键关联
		Object get(Object key)				返回指定key的value
		boolean containsKey()				是否包含key
		boolean containsValue()				是否包含value
		Set keySet()						返回此映射中包含的key的Set视图
		Collection<V> values()				返回此映射中包含的value的Collection视图
		Set<Map.Entry<K,V>>entrySet()		返回此映射中包含的映射关系的Set视图
		
##	6.5.2 HashMap集合
	
##	6.5.3 Properties集合