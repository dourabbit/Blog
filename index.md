---
layout: page
title: chengfu 的 Path-tracer
tagline: open source
---
{% include JB/setup %}

#关于渲染器， 关于path tracing
渲染器其实就是一个能把抽象的模型数据，经过复杂的数学和物理公式的计算，最后生成具体图像的软件。 它已经应用到我们生活的方方面面， 从console game到手机游戏， 电影到广告， 产品设计到室内家具设计。渲染器也是众多软件中价格高昂的之一。 同时，因为软件的算法强度特别高，更需要高昂的硬件投入，我们几乎所有制作公司都在用盗版。 这样造成了， 我们从教育到科研，甚至商业都高度依赖国外产品。

Path tracing算法是由James Kajiya在美国加州理工学院发表论文“The Rendering Equation”，首次引入到图形学中。James在论文中解决了渲染中的radiative transfer问题，并且提到几种variance-reduction的方法。Pathtracing属于Unbias算法, 同时Pathtracing的实现代码精简，也被很多大学做为课程。 

# ProjMeToo 目的

	创建一款针对教育的功能简洁的跨平台开源渲染器，并附有完善的中文文档

	projMeToo不是完整功能渲染器
	projMeToo不是单一平台开发
	projMeToo不是商业项目 禁止商业用途


# 进度
目前的版本为 0.1.0， 可以用效率较高的path tracing计算cornel box。 但是模型和ray-geometry collision test 还是用的参数方程.

#渲染公式

$L_{o}(x,\Theta_{o}) 	= L_e(x,\Theta_{o}) + L_r(x,\Theta_o) = L_e(x,\Theta_{o}) + \int_{\pi_i}L_i(x,\Theta_i)f_r(x,\Theta_i,\Theta_o) |\Theta_i \cdot N_x| d\omega_i$

以上公式表达了一个表面的Radiance等同于自身的Radiance(self-emitted radiance)和反射的radiance(reflected radiance)之和。
 同时， BRDF函数（$f_r$）定义了光线从不透明表面反射程度。

$L_{o}(x,\Theta_{o}) 	＝L_e(x,\Theta_{o}) + \int_{\pi_i}L_o(y,\Theta_i)f_r(x,\Theta_i,\Theta_o) |\Theta_i \cdot N_x| d\omega_i$

通过引入点y： $y= ray(x,\Theta_i^{-1})$ 我们可以吧半球中所有的方向$\pi^{-1}$的Ray做积分

$L_{o}(x,\Theta_{o}) 	＝L_e(x,\Theta_{x}) + \int_{\pi^{-1}}L(y,\Theta_y)f_r(x,\Theta_y,\Theta_x) |\Theta_y \cdot N_x| d\omega_y$

这样渲染公式就转换成了$y\rightarrow x$,2点之间的公式。（未完待续）

#Monte-Carlo
Monte Carlo 是一种随机采样求积分的方法。 例如， f(x)再域a到b上的的积分为
$I= \int_a^bf(x)dx$

最直接的做法是求的f平均数值，然后乘以b－a；同样我门在域上uniformly distributed random 采样点 $\xi_1,\xi_2,\xi_3,\xi_N$, 这样得出：

$I_m = (b-a)\frac{1}{N}\sum_{i=1}^Nf(\xi_i)$

在我们增加采样点得数目时候，Monte－Carlo estimator $I_m$会接近我门要求的$I$

$\lim_{N\rightarrow \infty} I_m = I$

estimator $I_m$ 的 variance为：

$\sigma^2 = \frac{1}{N}(\int_a^b f^2(x)dx-I^2)$

由此看出 $\sigma \propto \frac{1}{\sqrt N}$ 收敛的速度非常慢，但是对于rendering 高维度积分问题， 这样的速度比其他任何方法都快。(未完待续)

##uniformly sampling hemisphere
上文提出 uniformly distribution 是指 probability desity function 是个常数， 即 $p(\omega)=c$

$\int p(\omega)d\omega = 1$

$c\int d\omega = 1$

$c = \frac{1}{2\pi}$

由此得出 $p(\omega) = \frac{1}{2\pi}, p(\theta,\phi) = \frac{\sin\theta}{2\pi}$
需要求的$\theta$, 我门需要用marginal density function $p(\theta)$

$p(\theta) = \int_0^{2\pi}p(\theta,\phi)d\phi = \int_0^{2\pi} \frac{\sin\theta}{2\pi} d\theta = \sin\theta$

$p(\phi|\theta) = \frac{p(\theta,\phi)}{p(\theta)} = \frac{1}{2\pi}$

通过积分得出

$P(\theta) = \int_0^\theta \sin{\theta'} d\theta' = 1 - \cos\theta$

$P(\phi|\theta) = \int_0^\phi \frac{1}{2\pi} d\phi' = \frac{\phi}{2\pi}$

然后1D inversion, 同时利用对称性得出：

$\theta = \cos^{-1} \xi_1$

$\phi = 2\pi\xi_2$


#算法

# 视频

# Appendix

### Appendix 数学例子

### Appendix 资料

### Appendix pseudo-code

	// global illumination algorithm
	// stochastic ray tracing
	computeImage(eye)
		for each pixel
			radiance = 0;
			H = integral(h(p));
			for each sample // Np viewing rays
				pick sample point p within support of h;
				construct ray at eye, direction p-eye;
				radiance = radiance + rad(ray)*h(p);
			radiance = radiance/(#samples*H);

	rad(ray)
		find closest intersection point x of ray with scene;
		return(Le(x,eye-x) + computeRadiance(x, eye-x));

	computeRadiance(x, dir)
		estimatedRadiance += directIllumination(x, dir);
		estimatedRadiance += indirectIllumination(x, dir);
		return(estimatedRadiance);

	directIllumination (x, theta)
		estimatedRadiance = 0;
		for all shadow rays // Nd shadow rays
			select light source k;
			sample point y on light source k;
			estimated radiance +=
				Le * BRDF * radianceTransfer(x,y)/(pdf(k)pdf(y|k)); stimatedRadiance = estimatedRadiance / #paths;
		estimatedRadiance = estimatedRadiance / #paths;
		return(estimatedRadiance);

	indirectIllumination (x, theta)
		estimatedRadiance = 0;
		if (no absorption) // Russian roulette
			for all indirect paths // Ni indirect rays
				sample direction psi on hemisphere;
				y = trace(x, psi);
				estimatedRadiance +=
					compute_radiance(y, -psi) * BRDF *
					cos(Nx, psi)/pdf(psi);
			estimatedRadiance = estimatedRadiance / #paths;
		return(estimatedRadiance/(1-absorption));
	
	radianceTransfer(x,y)
		transfer = G(x,y)*V(x,y);
		return(transfer);

### Appendix To-do list

	Path tracing 算法                                0.1.x (we are here)
	离散几何体                                        0.2.x
	Ray geometry优化                                 0.3.x
	Texture                                          0.4.x
	Shading API                                      0.5.x
	Sampler                                          0.6.x	

	MLT						–－－－	1.1.x	

### Appendix 各种平台运行速度比较

### Appendix 感谢
	龙芯集团提供了3个月的电脑
	
