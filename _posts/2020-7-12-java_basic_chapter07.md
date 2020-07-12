---
layout: post
title: "java基础案例教程--第七章: IO（输入输出）"
date: 2020-7-12 
description: "Java"
tags: "java基础案例教程"
---


IO流： 按照操作数据不同，可分为：字节流和字符流
		按照数据传输方向不同，可分为： 输入流和输出流

# 7.1 字节流

	7.1.1 字节流的概念
		在计算机中，无论是文本、图片、音频还是视频，所有文件都是以二进制（字节）形式存在的，
		IO流中针对字节的输入输出提供了一系列的流，统称为字节流。
		在JDK中，提供了两个抽象类InputStream和OutputStream。
		InputStream的常用方法：
			int read() 从输入流杜宇一个8位的字节，把它转换为0-255之间的整数，并返回这一整数。
			int read(byte[] b)	从输入流读取若干字节，把它们保存到参数b指定的字节数组中，返回的整数表示读取字节的数目。
			int read(byte[] b, int off, int len) 	从输入流读取若干字节，把他们保存到参数b指定的字节数组中，
													off指定字节数组来时保存数据的起始下标，len表示读取的字节数目
			void close()	关闭此输入流并释放与该流关联的所有系统资源
		OutputStream的常用方法
			write()	写入
			flush()	刷新此输出流并强制写出所有缓冲的输出字节
			close()	关闭此世俗出流并释放与此流相关的所有系统资源
	7.1.3 文件的拷贝
	7.1.4 字节流的缓冲区
	7.1.5 字节缓冲流
			BufferedInputStream和BufferedOutputStream

# 7.2 字符流

# 7.3 File类