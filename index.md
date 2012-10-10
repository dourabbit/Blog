---
layout: page
title: chengfu 的 Pathtracing
tagline: open source
---
{% include JB/setup %}

#关于渲染器， 关于pathtracing
关于渲染器的定义，大家可以去wiki一下。这里我只给出一些个人的看法。 渲染器其实就是一个能把抽象的模型数据，经过复杂的数学和物理公式的计算，最后生成具体图像的软件。 它已经应用到我们生活的方方面面， 从console game到手机游戏， 从电影到广告， 从产品设计到室内家具设计。渲染算法是图形学算法中最重要部分之一。 渲染器也是众多软件中价格高昂的之一。 同时，因为软件的算法强度特别高，更需要高昂的硬件投入，中国（除香港，澳门，以及台湾）几乎所有制作公司都在用盗版。 这样造成了， 我们从教育到科研再到商业都高度依赖国外产品。

Pathtracing是最古老的全局光照算法， 是美国加州理工学院 教授在1983年发表论文中提出的算法。 Pathtracing属于Unbias算法， 适合于设计领域。 Pathtracing的实现代码精简， 同时也被美国多所著名大学作为图形学课程的作业。 
$e^x = \sum_{n=0}^\infty \frac{x^n}{n!} = \lim_{n\rightarrow\infty} (1+x/n)^n$
# ProjMeToo 目的

	创建一款针对教育的功能完整的跨平台开源渲染器，并附有完善的中文文档

	projMeToo不是产品级渲染器
	projMeToo不是单一平台开发
	projMeToo不是商业项目 所有代码遵循MIT License 对于商业项目 作者保留所有权利
	

# 问题
path tracing， bidirectional tracing & Metropolis light transporting 利弊个是什么？那个更能解决通用问题

# 进度
目前的版本为 0.1.0， 可以用效率较高的path tracing计算cornel box。 但是模型和ray-geometry collision test 还是用的参数方程.

# 数学

# 算法

# 视频

# Appendix

### Appendix 数学例子

### Appendix 资料

### Appendix pseudo-code

### Appendix To-do list

	Path tracing 算法			－－－ 	0.1.x (we are here)
	离散几何体				－－－	0.2.x
	Ray－geometry优化			－－－	0.3.x
	Texture					－－－	0.4.x
	Shading API				－－－	0.5.x
	Sampler					－－－	0.6.x

### Appendix 各种平台运行速度比较

### Appendix 感谢
	龙芯集团提供了3个月的电脑
	
