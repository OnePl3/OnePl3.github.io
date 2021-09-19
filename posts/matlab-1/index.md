# Matlab 1


# Matlab 1：Fourier Analysis Basic

 (Matlab实现)
<!--more-->

### 实现目标

​	利用傅里叶级数思想：每一个函数都可以展开为傅里叶级数（周期函数/非周期函数），通过不断叠加具有不同的周期的三角函数，可以使得叠加结果逐渐逼近目标函数。

### 实现思路

​	从向量空间的角度来对傅里叶级数进行理解，N维空间中的向量可以分解成N个基向量的线性组合。



![基向量](/images/%E5%9F%BA%E5%90%91%E9%87%8F.png)
$$
f(x) = \frac{{{A_0}}}{2} + \sum\limits_{k = 1}^\infty  {({A_k}\cos (\frac{{\pi kx}}{l}) + {B_k}\sin (\frac{{\pi kx}}{l}))}
$$
​	把傅里叶级数看成是空间里的某个向量，然后我们再定义一个内积，让这些基向量是正交单位基向量，那么傅里叶系数就可以写成如下的形式：


$$
{A_0} = \frac{1}{l}\int_{ - l}^l {f(x)dx}  =  < f(x),1 > *\Delta x
$$

$$
{A_k} = \frac{1}{l}\int_{ - l}^l {f(x)\cos (\frac{{\pi kx}}{l})dx}=  < f(x),\cos (\frac{{k\pi x}}{l}) > *\Delta x
$$


$$
{B_k} = \frac{1}{l}\int_{ - l}^l {f(x)\sin (\frac{{\pi kx}}{l})dx}  =  < f(x),\sin (\frac{{k\pi x}}{l}) > *\Delta x
$$

### 实现代码

在代码实现部分通过初始拟合20次叠加来贴合目标函数，后续若还需拟合其他函数，仅需调制目标函数f（x）即可对其进行拟合

```matlab
%{
利用傅里叶变换拟合各类波形
%}

clear all, close all, clc     %clear 清除变量

% Define domain
dx = 0.001;
L = pi;
x = (-1+dx:dx:1)*L; %dx为步长
n = length(x);   nquart = floor(n/4);
%Define hat function
f = 0*x;%初始0
f(nquart:2*nquart) = 4*(1:nquart+1)/n;
f(2*nquart+1:3*nquart) = 1-4*(0:nquart-1)/n;
plot(x,f,'-k','LineWidth',1.5), hold on

% Compute Fourier series
CC = jet(20);%控制颜色
% A0 = sum(f.*dx);
A0 = sum(f.*ones(size(x)))*dx;
fFS = A0/2;
for k=1:20
    A(k) = sum(f.*cos(pi*k*x/L))*dx; % Inner product
    B(k) = sum(f.*sin(pi*k*x/L))*dx;
    fFS = fFS + A(k)*cos(k*pi*x/L) + B(k)*sin(k*pi*x/L);
    plot(x,fFS,'-','Color',CC(k,:),'LineWidth',2)
    pause(.1)
end
```



### 可视效果

![MPhoto](/images/MPhoto.png)






