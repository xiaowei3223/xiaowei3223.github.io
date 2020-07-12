---
layout: post
title: "java基础案例教程--第八章：GUI（图形用户界面）"
date: 2020-7-12 
description: "Java"
tags: java基础案例教程
---

**GUI全称是Graphical User Interface,即图形用户界面，就是应用程序提供给用户操作的图形界面，
包括窗口、菜单、按钮、工具栏和其他各种图形界面元素。
JAVA中提供java.awt和java.swing类库来设计GUI,简称为AWT和Swing**

# 8.1 AWT概述
	AWT是用于创建图形用户界面的一个工具包，它提供了一系列用于实现图形界面的组件，如窗口、按钮、文本框、
	对话框等。
	
	AWT通常分为两大类：
	
		- Component: 除菜单外其他AWT组件的父类，分为基本组件类和容器类。
		- MenuComponent：所有与菜单相关组件的父类
	
	1. Window
	
		Window类是不依赖其他容器二独立存在的容器，有两个子类：
		
			- Frame类： 用于创建一个具有标题栏的框架窗口，作为程序的主界面。
			- Dialog类： 用于创建一个对话框，实现与用户的信息交互。
	
	2. Panel
	
		Panel也是一个容器，不能单独存在，只能存在与其他容器（Window或其子类）中，一个Panel对象代表了一个
		长方形的区域，在这个区域中可以容纳其他组件。
		通常使用Panel来实现一些特殊的布局。

# 8.2 布局管理器
	java.awt包提供了5中布局管理器：
		- FlowLayout(流式布局管理器)
		- BorderLayout(边界布局管理器)
		- GridLayout(网格布局管理器)
		- GridBagLayout(网格包布管理器)
		- CardLayout(卡片布局管理器)

##	8.2.1 FlowLayout
		FlowLayout是最简单的布局管理器，将组件按照添加顺序从做向右放置。
		当到达容器的边界，会自动将组件放到下一行的开始位置。
		FlowLayout构造方法
			
			- `FlowLayout()`	组件默认居中对齐，水平、垂直间距默认为5个单位
			- `FlowLayout(int align)` 	指定组件向对于容器的对其方式，水平、垂直间距默认为5个单位
			- `FlowLayout(int align, int hgap, int vgap)` 指定组件的对其方式和水平、垂直间距
			
		Flow布局管理其的特带你就是可以将所有组件向流水一样一次进行排列，不需要用户明确地设定，但是在灵活性上相对差了点。

##	8.2.2 BorderLayout
		BorderLayout将容器分为5个区域，分别为东(EAST)南(SOUTH)西(WEST)北(NORTH)中(CENTER)
		`add(Component comp, Object constraints)` 向BorderLayout添加组件，
												comp表示要添加的组件，
												constraints指定将组件添加到布局中的方式和位置的对象
												
##	8.2.3 GridLayout
		GridLayout网格布局管理器使用中横线将容器分成n行m列大小相等的网格，每个网格放一个组件。
		GridLayout构造方法：
		- GridLayout() 默认只有一行，每个组件占一列
		- GridLayout(int rows, int cols) 指定容器的行数和列数
		- GridLayout(int rows, int cols, int hgap, int vgap) 指定容器的行数和列数以及组件之间的水平、垂直间距
		
##	8.2.4 GridBagLayout
		- GridBagLayout网格包布管理器是最灵活、最复杂的布局管理器。
		- GridBagLayout允许网格中的组件大小各不相同，而且运行一个组件跨越一个或多个网格。
		- 使用GridBagLayout：
			1. 创建GridBagLayout布局管理器，并使用容器采用该布局管理器。
				GridBagLayout layout = new GridBagLayout();
				container.setLayout(layout);
			2. 创建GridBagContraints对象（布局约束条件），并设置该对象的相关属性。
				GridBagConstraints constraints = new GridBagConstraints();
				constraints.gridx = 1;	//设置网格的左上角横向索引
				constraints.gridy = 1; 	//设置网格的左上角纵向索引
				constraints.gridwidth = 1; //设置组件横向跨越的网格
				constraints.gridheight = 1; //设置组件纵向跨越的网格
				
			3. 调用GridBagLayout对象的setConstraints()方法建立GridBagConstrains对象和受控组件之间的关联。
				layout.setConstrains(component, constraints);
			4. 向容器中添加组件
				container.add(conponent); 
		GridBagConstraints常用属性：
		gridx和gridy 			设置组件的左上角所在网格的横向和纵向索引（即所在的行和列）
		gridwidth和gridheight  	设置组件横向/纵向跨越几个网格
		fill 					如果组件的显示区域大于组件需要的大小，设置是否改变组件大小，有以下几个值：
									NONE：默认
									HORIZONTAL： 是组件水平方向足够长以填充显示区域，但是高度不变
									VERTICAL: 使组件垂直方向足够高以填充显示区域，但长度不变
									BOTH: 使组件足够大，以填充整个显示区域
		weightx和weighty 		设置组件占领容器中多余的水平方向和垂直方向空白的比例（也称为权重）。
		
##	8.2.5 CardLayout
		在操作程序时，经常会遇到通过选项卡按钮来切分程序中的界面，这些界面就相当于一张张卡片，而管理这些
		卡片的布局管理器就是卡片布局管理器CardLayout。
		CardLayout常用方法：						
			first(Container parent)					显示parent容器的第一张卡片
			last(Container parent)					显示parent容器的最后一张卡片
			previous(Container parent)				显示parent容器的前一张卡片	
			next(Containerparent)					显示parent容器的下一张卡片
			show(Container parent, String name)		显示parent容器中名称为name的组件，如果不存在，则不会发生任何操作
			
##	8.2.6 不适用布局管理器
		当一个容器被创建后，他们都会有一个默认的布局管理器。
		Window、Frame和Dialog的默认布局管理器时BorderLayout。
		Panel的默认管理器时FlowLayout。
		如果不希望通过布局管理器来对容器进行布局，也可以调用容器的setLayout(null)方法将布局管理器取消。
		同时，必须调用容器中每个组件的setSize()和setLocation()或者是setBounds()来为这些组件在容器中定位。
		
# 8.3 AWT事件处理

##	8.3.1 事件处理机制
		事件处理机制专门用于相应用户的操作，比如：想要相应用户的单机鼠标、按下键盘等操作，就需要使用AWT的事件处理机制。
		事件对象(Event)  封装了GUI组件上发生的特定事件（通常就是用户的一次操作）
		事件源（组件）	 事件发生的场所，通常就是产生事件的组件。
		监听器(Listener) 负责监听事件源上发生的事件，并对各种事件做出相应处理的对象（对象中包含事件处理器）
		事件处理器		 监听器对象对接受的事件对象进行相应处理的方法。
		
##	8.3.2 事件适配器
		JDK提供了一些适配器类，它们是监听器接口的默认实现类，这些实现类中实现类接口的所有方法，
		但方法中没有任何代码，程序可以通过继承适配器类来达到实现监听器接口的目的。
		
##	8.3.3 用匿名内部类实现事件处理
		为了代码的简介，经常通过匿名内部类来创建事件的监听器对象，针对所发生的事件进行处理。
		
# 8.4 常用事件分类
	AWT提供了丰富的事件，这些事件大致可以分为：
		窗体事件(WindowEvent)
		鼠标事件(MouseEvent)
		键盘事件(KeyboardEvent)
		动作事件(ActionEvent)
		
##	8.4.1 窗体事件
		窗体事件： 窗体的打开、关闭、激活、停用等都属于窗体时间见。
		当对窗体事件进行处理是，首先需要定义一个实现WindowListenter接口的类作为拆个难题监听器，然后
		通过addWindowListener()方法将窗体对象与窗体监听器绑定。
		
##	8.4.2 鼠标事件(MouseEvent)
		鼠标事件包括：鼠标按下、鼠标松开、鼠标单击等。
		JDK提供了一个MousEvent类用于表示鼠标事件，几乎所有的组件都可以产生鼠标事件。
		处理鼠标事件时：
			首先需要通过实现MouseListener接口定义监听器（也可以通过继承适配器MouseAdapter类来实现），
			然后调用addMouseListener()方法将监听器绑定到事件源对象。
			
##	8.4.3 键盘事件(KeyboardEvent)
		JDK在提供了一个KeyEnent类表示键盘事件，处理keyEvent事件的监听器对象需要实现Keylistener接口或者继承KeyAdapter类。
		
##	8.4.4 动作事件(ActionEvent)
		动作事件与前面三种事件有所不同，它不代表某个具体的动作，只是表示一个动作发生了。
		在java中，动作事件用ActionEvent类表示，处理ActionEvent事件的监听器对象需要实现ActionListener接口。
		
# 8.5 AWT绘图
	Graphics类，是专门用来在GUI组件上绘图的。
	setColor(Color c) 	将此图形上下文的当前颜色设置为指定颜色
	setFont(Font f)		将此图形上下文的字体设置为指定字体
	drawLine(int x1, int y1, int x2, int y2) 	以（x1,y1)和(x2,y2)为端点绘制一条线条
	drawRect(int x1, int y1, int width, int height)	绘制指定矩形的边框
	drawOval(int x1, int y1, int width, int height)	绘制椭圆的边框
	fillRect(int x1, int y1, int width, int height)	用当前颜色填充于指定的矩形
	fillOval(int x1, int y1, int width, int height) 用当前颜色填充于指定的椭圆
	drawString(String str, int x, int y)	使用此图形上下文的当前字体和颜色绘制指定的文本str
	
# 8.6 Swing
大部分Swing组件都是JComponent类直接或间接子类， 而JComponent类是AWT中Java.awt.Container的子类。
Swing中有三个组件（JWindow、JFrame、Jdialog）继承了AWT的Window类，而不是继承子JComponent类。

##	8.6.1 JFrame
		JFrame类提供了关闭窗口的功能，在程序中不需要添加窗体监听器，只需要调用setDefaultCloseOperation()方法，
		然后将常量JFram.EXIT_ON_CLOSE作为参数传入即可。

##	8.6.2 JDialog
		JDialog与Dialog一样都表示对话框。JDialog对话框可以分为两种：
			模态对话框： 是指用户需要等到处理完对话框后才能继续与其他窗口交互的对话框。
			非模态对话框：允许用户在处理对话框的同时与其他窗口交互的对话框。
		JDialog构造方法
		Jdialog(Frame owner)	创建一个非模态的对话框， owner为对话框所有者（顶级窗口JFrame）
		JDialog(Frame owner, String title)	创建一个具有指定标题的非模式对话框
		JDialog(Frame owner, boolean  model)	创建一个有指定模式的无标题对话框

##	8.6.3 中间容器
		中间容器有两种：
			JPanel：和AWT中的Panel组件使用方法基本一致，
					是一个无边框，不能被移动、放大、缩小或者关闭的面板。
					默认布局管理器是FlowLayout.
			JScrollPane： 是一个带有滚动条的面板容器，只能添加一个组件。
						  如果向在JScrollPane面板中添加多个组件，应该先将组件添加到JPanel中，然后将JPanel添加到JScrollPane
		JScrollPane构造方法：
			JScrollPane()	创建一个空的JScrollPane面板
			JScrollPane(Component view) 	创建一个显示指定组件的的JScrollPane面板
											只要组件的内容超过视图大小就会显示水平和垂直滚动条
			JScrollPane(Component view, int vsbPloicy, int hsbPloicy)	创建一个显示指定组件并具有指定滚动条策略的JScrollPane面板
																		vsbPloicy和hsbPloicy分别表示垂直和水平滚动条策略

##	8.6.4 文本组件
		文本组件用于接受用户输入的信息或向用户展示信息其中包括：
			文本框（JTextField）
			文本域（JtextArea）
		它们都有一个共同的父类：JTextComponent
		JTextComponent常用方法：
			String getText()			返回文本组件中所有的文本内容
			String getSelectedText()	返回文本组件中选定的文本内容
			void selectAll()			在文本组件中选中所有内容
			void setEditable()			设置文本组件为可编辑或者不可编辑状态
			void setText(String text)	设置文本组件的内容
			void replaceSelection(String content)	用给定的内容替换当前选定的内容
		文本框 JTextField： 只接受单行文本的输入。
			JTextField()							创建一个空的文本框，初始字符串为null
			JTextField(int columns)	创建一个具有指定列数的的文本框，初始字符串为null
			JTextField(String text)	创建一个显示指定初始字符串的文本框
			JTextField(String text, int columns)	创建一个具有指定列数并显示指定初始字符串的文本框
		JTextField有一个子类JPasswordText，
			表示一个密码框，
			只能接受用户的单行输入，
			但是在此框中不显示用户输入的真实信息。
		JTextArea: 能接受多行的文本输入，使用JTextArea构造方法创建对象时可以设定区域的行数、列数。
			JTextArea()		创建一个空的文本域
			JTextArea(String text)	创建显示指定初始字符串的文本域
			JTextArea(int rows, int columns)	创建具有指定行和列的空的文本域
			JTextArea(String text, int rows, int columns)	创建显示指定初始文本并指定了行列的文本域

##	8.6.5 按钮组件
		在Swing中，常见的按钮组件有JButton、JCheckBox、JRadioButton等，它们都是抽象类AbstractButton类的直接或间接子类
		AbstractButton常用方法
			Icon getIcon()和void setIcon(Icon icon)		设置或者获取按钮的图标
			String getText()和void setText(String text)	设置或者获取按钮的文本
			void setEnable(boolean b)	启用（当b为true）或禁用（当b为false）按钮
			setSelected(boolean b)	设置按钮状态，当b为true，按钮是选中状态
			boolean isSelected()	返回按钮的状态
		1. JCheckBox 复选框
			JCheckBox构造方法
			JCheckBox()	创建一个没有文本信息，初始状态未被选中的复选框
			JCheckBox(String text)	创建一个带有文本信息，初始状态未被选中的复选框
			JCheckBox(String text, boolean selected) 创建一个带有文本信息，并指定初始状态（选中/未选中）的复选框
		2. JRadioButton 单选按钮
			JRadioButton构造方法：
				JRadioButton()	创建一个没有文本信息、初始化状态未被选中的单选框
				JRadioButton(String text) 创建一个带有文本信息、初始化状态未被选中的单选框
				JRadioButton(String text, boolean selected) 创建一个带有文本信息、指定初始化状态（选中/未选中）的单选框

##	8.6.6 JComboBox 下拉列表框（组合框）
		JComboBox下拉列表框将所有选项折叠收藏在一起，默认显示的是第一个添加的选项。当用户单击组合框时，
		会出现下拉式的选择列表，用户可以从中选择其中一项并显示。
		
		JComboBox组件框组件分为两种形式：
			可编辑：用户可以在现有的选项中选择，也可以自己输入新的内容
			不可编辑：用户只能在现有的选项列表中进行选择
		JComboBox构造方法：
			JComboBox()		创建一个没有可选项的组合框
			JComboBox(Object[] items)	创建一个组合框，将Object数组中的元素作为组合框的下拉列表选项
			JComboBox(Vector items)		创建一个组合框，将Vetor集合中的元素作为组合框的下拉列表选项
		JComboBox常用方法：
			void addItem(Objcet anObject)		为组合框添加选项
			void insertItemAt(Objcet anObject, int index) 	在指定的索引处插入选项
			Object getItemAt(int index)	返回指定索引处选项，第一个选项的索引为0
			int getItemCount()	返回组合框中选项的数目
			Object getSelectedItem()	返回当前所选项
			void removeAllItems()	删除组合框中所有选项
			void removeItem(Object object)	从组合框中删除指定选项
			void removeItemAt(int index)	移除指定索引处的选项
			void setEditable(boolean aFlag)	设置组合框中的选项是否可编辑，aFlag为true则可编辑，反之则不可编辑

##	8.6.7 菜单组件
		1. 下拉式菜单
			下拉式菜单需要使用3个组件： 
				JmenuBar(菜单栏)	表示一个水平的菜单栏，用来管理菜单，不参与同用户的交互式操作。
									可以放置在容器的任何位置， 
									但通常情况下会使用顶级窗口（如JRFrame、Jdialog)的setJmenuBar(JMenuBar menuBar)方法
										将它放置在顶级窗口的顶部。
									
				Jmenu(菜单)		用来整合管理菜单项，菜单可以是单一层次的结构，也可以是多层次的结构。
				JmenuItem(菜单项)	是菜单系统中最基本的组件。
		2. 弹出式菜单
			弹出是菜单用JPopupMenu表示。
##	8.6.8 JTable
	
	