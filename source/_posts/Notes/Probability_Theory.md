---
title: 《概率论与数理统计》笔记
tags:
  - 笔记
categories: 学习笔记
mathjax: true
abbrlink: Note_Probability_Theory
swiper_index: 20
date: 2024-01-12 02:55:00
description: 考前3天速通99分概率论
---
# 一、随机事件与概率
## 1.公式：
1. 条件概率：$P(A|B)=\frac{P(AB)}{P(B)}$
2. 乘法公式：$P(AB)=P(A|B)P(B)=P(B|A)P(A)$
3. 全概率公式：$P(A)=\sum_{i=1}^nP(A|B_i)P(B_i)$
4. 贝叶斯公式：$P(B_i|A)=\frac{P(A|B_i)P(B_i)}{\sum_{i=1}^nP(A|B_i)P(B_i)}$
5. 抽签问题：$P(A)=\frac{m}{n}$

## 2.事件独立性
事件A,B,C相互独立 $\Leftrightarrow $ $P(ABC)=P(A)P(B)P\(C\),$ $P(AB)=P(A)P(B),$ $P(AC)=P(A)P\(C\),$ $P(BC)=P(B)P\(C\)$

### 例题
$A,B,C$独立，$ABC=\varnothing,$ $P(A)=P(B)=P\(C\)\lt\frac{1}{2},$ $P(A\cup B\cup C)=\frac{9}{16}$,求$P(A)$

$P(A\cup B\cup C)$ $=P(A)+P(B)+P\(C\)-P(AB)-P(AC)-P(BC)+P(ABC)$ $=3P(A)-3P(A)^2+0=\frac{9}{16}$ $\Rightarrow$ $P(A)=\frac{1}{4}$或$P(A)=\frac{3}{4}$(舍)

## 3.超几何分布
1. 概述：$n+m$件产品中有$m$件次品，从中抽取$k$件，求其中有$i$件次品的概率
2. 分布律：$P(X=i)=\frac{C_m^iC_{n}^{k-i}}{C_{m+n}^k}$

# 二、随机变量及其分布+数字特征
## 1. $X$ ~ $B(n,p)$
1. 分布名称：二项分布
2. 概述：$n$次独立重复试验中，事件A发生的次数$X$（伯努利试验）
3. 分布律：$P(X=k)=C_n^kp^k(1-p)^{n-k}$
4. 期望：$E(X)=np$
5. 方差：$D(X)=np(1-p)$

## 2. $X$ ~ $P(\lambda)$
1. 分布名称：泊松分布
2. 概述：多次试验中小概率事件A发生的次数$X$
3. 分布律：$P(X=k)=\frac{\lambda^k}{k!}e^{-\lambda}$
4. 期望：$E(X)=\lambda$
5. 方差：$D(X)=\lambda$
6. 泊松定理：对于$X$ ~ $B(n,p)$，当$n\rightarrow\infty,p\rightarrow0,np=\lambda$时，可近似认为$X$ ~ $P(\lambda)$

## 3. $X$ ~ $g\(p\)$
1. 分布名称：几何分布
2. 概述：多次试验中事件A第一次成功的次数$X$
3. 分布律：$P(X=k)=p(1-p)^{k-1}$
4. 期望：$E(X)=\frac{1}{p}$
5. 方差：$D(X)=\frac{1-p}{p^2}$
6. 无记忆性：$P(X>s+t|X>s)=P(X>t)$

## 4. $X$ ~ $U(a,b)$
1. 分布名称：均匀分布
2. 分布函数：$F(x)=\begin{cases}0&,x<a \\\\ \frac{x-a}{b-a}&,a\leq x\leq b \\\\ 1&,x>b\end{cases}$
3. 概率密度函数：$f(x)=\begin{cases}\frac{1}{b-a}&,a\leq x\leq b \\\\ 0&,\text{其他}\end{cases}$
4. 期望：$E(X)=\frac{a+b}{2}$
5. 方差：$D(X)=\frac{(b-a)^2}{12}$

## 5. $X$ ~ $E(\lambda)$
1. 分布名称：指数分布
2. 分布函数：$F(x)=\begin{cases}1-e^{-\lambda x}&,x\geq 0 \\\\ 0&,x<0\end{cases}$
3. 概率密度函数：$f(x)=\begin{cases}\lambda e^{-\lambda x}&,x\geq 0 \\\\ 0&,x<0\end{cases}$
4. 期望：$E(X)=\frac{1}{\lambda}$
5. 方差：$D(X)=\frac{1}{\lambda^2}$
6. $P(X>t)=e^{-\lambda t}$
7. 无记忆性：$P(X>s+t|X>s)=P(X>t)$
8. 与泊松分布的关系：服从$E(\lambda)$的事件，$t$时间内发生的次数$X$ ~ $P(\lambda t)$
9. 事件下次发生的时间：$X$ ~ $E(\lambda)$，$P(X>t)=e^{-\lambda t}$

## 6. $X$ ~ $N(\mu,\sigma^2)$
1. 分布名称：正态分布
2. 分布函数：$F(x)=\frac{1}{\sqrt{2\pi}\sigma}\int_{-\infty}^xe^{-\frac{(t-\mu)^2}{2\sigma^2}}dt$
3. 概率密度函数：$f(x)=\frac{1}{\sqrt{2\pi}\sigma}e^{-\frac{(x-\mu)^2}{2\sigma^2}}$
4. 期望：$E(X)=\mu$
5. 方差：$D(X)=\sigma^2$
6. 对称轴：$x=\mu$
7. 拐点：$x=\mu \pm \sigma$
8. 分布函数最大值：$f(x)_{max}=f(\mu)=\frac{1}{\sqrt{2\pi}\sigma}$
9. $\sigma\uparrow$：扁平 ； $\sigma\downarrow$：尖锐
10. $\mu\uparrow\downarrow$：左右平移，形状不变

## 7. $X$ ~ $N(0,1)$
1. 分布名称：标准正态分布
2. 分布函数：$\varPhi(x)=\frac{1}{\sqrt{2\pi}}\int_{-\infty}^xe^{-\frac{t^2}{2}}dt$
3. 概率密度函数：$\varphi(x)=\frac{1}{\sqrt{2\pi}}e^{-\frac{x^2}{2}}$
4. 期望：$E(X)=0$
5. 方差：$D(X)=1$
6. $\varPhi(-x)=1-\varPhi(x)$
7. $P(a<X<b)=\varPhi(b)-\varPhi(a)$
8. $P(|X|<a)=2\varPhi(a)-1$
9. $3\mu$原则：$P(|X-\mu|<k\sigma),k=1,2,3$分别对应$0.683,0.954,0.997$
10. 正态分布标准化：$X$ ~ $N(\mu,\sigma^2)$，$Z=\frac{X-\mu}{\sigma}$ ~ $N(0,1)$

# 三、多维随机变量及其分布
## 1. 二维随机分布函数的充要条件(3)
1. 右连续：$F(x+0,y)=F(x,y)$
2. 规范性：$P(+\infty,+\infty)=1,P(-\infty,-\infty)=0$
3. 容斥：$P(x_1\leq x\leq x_2,y_1\leq y\leq y_2)$ $=F(x_2,y_2)-F(x_1,y_2)-F(x_2,y_1)+F(x_1,y_1)$

## 2. 边缘分布函数
1. $F_X(x)=F(x,+\infty)$
2. $F_Y(y)=F(+\infty,y)$

## 3. 联合密度函数、边缘密度函数定义
1. 联合密度函数：$F(x,y)=\int_{-\infty}^x\int_{-\infty}^yf(x,y)dxdy$
2. 边缘密度函数：$f_X(x)=\int_{-\infty}^{+\infty}f(x,y)dy,f_Y(y)$ $=\int_{-\infty}^{+\infty}f(x,y)dx$

## 4. 二维正态分布
1. $(X,Y)$ ~ $N(\mu_1,\mu_2,\sigma_1^2,\sigma_2^2,\rho)$
2. 二维正态分布的边缘分布仍是正态分布：$X$ ~ $N(\mu_1,\sigma_1^2),Y$ ~ $N(\mu_2,\sigma_2^2)$
3. $X,Y$ 独立 $\Leftrightarrow$ $X,Y$ 无关 $\Leftrightarrow$ $\rho=0$

## 5. 可加性
1. 前提：$X_i$ 独立
2. 泊松分布：$X_i$ ~ $P(\lambda _i)$ $\Rightarrow$ $\sum X_i$ ~ $P(\sum \lambda _i)$
3. 二项分布：$X_i$ ~ $B(n_i,p)$ $\Rightarrow$ $\sum X_i$ ~ $B(\sum n_i,p)$
4. 正态分布：$X_i$ ~ $N(\mu _i,\sigma _i^2)$ $\Rightarrow$ $\sum k_iX_i$ ~ $N(\sum k_i\mu _i,\sum k_i^2\sigma _i^2)$

## 6. 最值分布函数
1. $F_{max}(x)=P(X_1\leq x,X_2\leq x,\cdots,X_n\leq x)$ $=\prod F(x_i)$
2. $F_{min}(x)=P(X_1\leq x,X_2\leq x,\cdots,X_n\leq x)$ $=1-\prod (1-F(x_i))$

### 例题：已知$\mathbb{X}=(X_1,X_2,\cdots,X_n)$独立同分布，且服从于$U(0,\theta)$，求$\max(\mathbb{X})$、$\min(\mathbb{X})$的密度函数$f_M(x)$、$f_N(x)$。
$X_i$ ~ $U(0,\theta)$ $\Rightarrow$ $F(x)=\begin{cases}0&,x<0 \\\\ \frac{x}{\theta}&,0\leq x\leq \theta \\\\ 1&,x>\theta\end{cases}$

$F_M(x)=\prod F(x_i)=(F(x))^n$ $=\begin{cases}0&,x<0 \\\\ \left(\frac{x}{\theta}\right)^n&,0\leq x\leq \theta \\\\ 1&,x>\theta\end{cases}$

$\Rightarrow$ $f_M(x)=\frac{dF_M(x)}{dx}=\begin{cases}\frac{nx^{n-1}}{\theta^n}&,0\leq x\leq \theta \\\\ 0&,Otherwise\end{cases}$

$F_N(x)=1-F_M(x)=\begin{cases}1&,x<0 \\\\ 1-\left(\frac{x}{\theta}\right)^n&,0\leq x\leq \theta \\\\ 0&,x>\theta\end{cases}$

$\Rightarrow$ $f_N(x)=\frac{dF_N(x)}{dx}=\begin{cases}\frac{nx^{n-1}}{\theta^n}&,0\leq x\leq \theta \\\\ 0&,Otherwise\end{cases}$

# 专：随机变量函数的分布
## 1. 离散型随机变量
1. 一维：$P(X=x_i)=p_i$，$Y=h(X)$ $\Rightarrow$ $P(Y=y_j)=\sum\limits_{h(x_i)=y_j}p_i$
2. 二维：$P(X=x_i,Y=y_j)=p_{ij}$，$Z=h(X,Y)$ $\Rightarrow$ $P(Z=z_k)=\sum\limits_{h(x_i,y_j)=z_k}p_{ij}$

## 2. 连续型随机变量
### 一维例题：$X$ ~ $N(0,1)$ , $Y=X^2$，求$f_Y(y)$
第1步：写$X$的取值范围，确认$f_Y(y)$的定义域，定义域外$f_Y(y)=0$

$X\in R$ $\Rightarrow$ $Y=X^2\geq 0$

第2步：写出$X$的分布函数$F_X(x)$

$F_X(x)=\varPhi(x)$

第3步：求$Y$的分布函数$F_Y(y)$

$y\geq 0$ 时， $F_Y(y)=P(Y\leq y)=P(X^2\leq y)$ $=P(|X| \leq \sqrt{y})=2\varPhi(\sqrt{y})-1$

$\Rightarrow$ $F_Y(y)=\begin{cases}0&,y<0 \\\\ 2\varPhi(\sqrt{y})-1&,y\geq 0\end{cases}$

第4步：对$F_Y(y)$求导，得$f_Y(y)$

$f_Y(y)=\frac{dF_Y(y)}{dy}=\begin{cases}0&,y<0 \\\\ y^{-\frac{1}{2}}\varphi(\sqrt{y})&,y\geq 0\end{cases}$

### 二维例题：$X$ ~ $E(1)$ , $Y$ ~ $U(0,1)$ , $X,Y$独立 , $Z=X+2Y$，求$f_Z(z)$
第1步：写$X,Y$的取值范围，确认$f_Z(z)$的定义域，定义域外$f_Z(z)=0$

$X \geq 0$ , $Y\in [0,1]$ $\Rightarrow$ $Z=X+2Y\geq 0$

第2步：写出$X,Y$的密度函数$f_X(x)$、$f_Y(y)$

$f_X(x)=\begin{cases}e^{-x}&,x\geq 0 \\\\ 0&,Otherwise\end{cases}$

$f_Y(y)=\begin{cases}1&,y\in[0,1] \\\\ 0&,Otherwise\end{cases}$

第3步：写出$X,Y$的联合密度函数$f_{XY}(x,y)$

$f_{XY}(x,y)=f_X(x)f_Y(y)=\begin{cases}e^{-x}&,x\geq 0,y\in[0,1] \\\\ 0&,Otherwise\end{cases}$

第4步：确定积分区域（作图）

第5步：求$Z$的分布函数$F_Z(z)$

$F_Z(z)=P(Z\leq z)=P(X+2Y\leq z)=P(Y\leq \frac{z-X}{2})$ 

$$\begin{align}
\Rightarrow F_Z(z)&=\begin{cases}
    \int_0^ze^{-x}dx\int_0^{\frac{z-x}{2}}dy &, z\in[0,2]   \\\\ 
    \int_0^{z-2}e^{-x}dx\int_0^1dy+\int_{z-2}^ze^{-x}dx\int_0^{\frac{z-x}{2}}dy &, z\geq 2  \\\\ 
    0 &, Otherwise
\end{cases}
 \\\\ 
&=\begin{cases}
    \frac{1}{2}(e^{-z}+z-1) &, z\in[0,2]  \\\\ 
    1-\frac{1}{2}e^{2-z}+\frac{1}{2}e^{-z} &, z\geq 2  \\\\ 
    0 &, Otherwise
\end{cases}
\end{align}
$$

第6步：对$F_Z(z)$求导，得$f_Z(z)$

$$
f_Z(z)=\frac{dF_Z(z)}{dz}=
\begin{cases}
    -\frac{1}{2}e^{-z}+\frac{1}{2} &, z\in[0,2]  \\\\ 
    \frac{1}{2}e^{2-z}-\frac{1}{2}e^{-z} &, z\geq 2  \\\\ 
    0 &, Otherwise
\end{cases}
$$

# 四、随机变量的数字特征
## 1. 数学期望
离散型：$E(X)=\sum x_ip_i$

连续型：$E(X)=\int_{-\infty}^{+\infty}xf(x)dx$

随机变量函数：$E(Y)=E(h(X))$

性质：
1. $E\(C\)=C$
2. $E(CX)=CE(X)$
3. $E(X\pm Y)=E(X)\pm E(Y)$
4. $X,Y$ 独立 $\Rightarrow$ $E(XY)=E(X)E(Y)$

## 2. 方差
$D(X)=E(X-E(X))^2=E(X^2)-[E(X)]^2$

标准差：$\sqrt{D(X)}$

性质：
1. $D\(C\)=0$
2. $D(X+C)=D(X)$
3. $D(CX)=C^2D(X)$
4. $D(X\pm Y)=D(X)+D(Y)\pm 2Cov(X,Y)$
5. $X,Y$ 独立 $\Rightarrow$ $D(X\pm Y)=D(X)+D(Y)$
6. $D(X)=0 \Leftrightarrow P(X=E(X))=1$

## 3. 协方差
$Cov(X,Y)=E(XY)-E(X)E(Y)$

性质：
1. $Cov(X,X)=D(X)$
2. $Cov(X,Y)=Cov(Y,X)$
3. $Cov(aX,bY)=abCov(X,Y)$
4. $Cov(X_1+X_2,Y)=Cov(X_1,Y)+Cov(X_2,Y)$
5. $Cov(X,C)=0$
6. $X,Y$ 独立 $\Rightarrow$ $Cov(X,Y)=0$

## 4. 相关系数
随机变量$X$的标准化：$X^*=\frac{X-E(X)}{\sqrt{D(X)}}$

相关系数：$\rho_{XY}=Cov(X^\*,Y^\*)=\frac{Cov(X,Y)}{\sqrt{D(X)}\sqrt{D(Y)}}$

性质：
1. $|\rho_{XY}|\leq 1$
2. $|\rho_{XY}|=1 \Leftrightarrow$ $X,Y$ 成线性关系，$\exists a,b\ s.t.\ P(Y=aX+b)=1$, $a\rho_{XY}>0$

# 五、大数定律与中心极限定理
## 1. 大量独立同分布随机变量和的极限分布是正态分布（基础）
$E(X_i)=\mu,D(X_i)=\sigma^2,Y=\sum X_i$

$\Rightarrow$ $E(Y)=n\mu,D(Y)=n\sigma^2$

$\Rightarrow$ $Y$ ~ $N(n\mu,n\sigma^2)$

$\Rightarrow$ 标准化： $\frac{Y-n\mu}{\sigma\sqrt{n}}$ ~ $N(0,1)$

## 2. 切比雪夫不等式（重点）
若$E(X)=\mu,D(X)=\sigma^2$，则对任意$\epsilon>0$，有$P(|X-\mu|\geq \epsilon)\leq \frac{\sigma^2}{\epsilon^2}$

落在$\mu\pm\epsilon$外的概率不超过$\frac{\sigma^2}{\epsilon^2}$

## 3. 中心极限定理
$\mathbb{X}=(X_1,X_2,\cdots,X_n)$独立同分布，$E(X_i)=\mu,0\lt D(X_i)=\sigma^2\lt\infty$，则$\forall x,\lim\limits_{n\rightarrow\infty}P\left(\frac{\sum X_i-n\mu}{\sigma\sqrt{n}}\leq x\right)=\varPhi(x)$

### 例题：每台车床有$70\%$的时间在工作
### 1. 100台车床，求任意时刻有70至80台车床在工作的概率
第1步：写出单个样本$X$的期望和方差

单个车床$X$ ~ $B(1,0.7)$ $\Rightarrow$ $E(X)=0.7,D(X)=0.21$

第2步：写出样本总体$Y$的期望和方差

记任意时刻工作的车床数为$Y$，
$E(Y)=100E(X)=70,D(Y)=100D(X)=21$

第3步：写出所求概率，对$Y$进行标准化，并用标准正态分布分布函数表示

$P(70\leq Y\leq 80)=P\left(\frac{70-70}{\sqrt{21}}\leq \frac{Y-70}{\sqrt{21}}\leq \frac{80-70}{\sqrt{21}}\right)$ $=\varPhi\left(\frac{10}{\sqrt{21}}\right)-\varPhi(0)=\varPhi\left(2.18\right)-\varPhi(0)$

第4步：查表代入

### 2. 求以0.997的概率保证 任意时刻至少有80台车床在工作 所需的车床数
设所需车床数为$N$，任意某时刻工作的车床数为$X$，则$E(X)=0.7N,D(X)=0.21N$

$P(X\geq 80)=1-P(X\lt 80)$ $=1-P\left(\frac{X-0.7N}{\sqrt{0.21N}}\lt \frac{80-0.7N}{\sqrt{0.21N}}\right)$ $=1-\varPhi\left(\frac{80-0.7N}{\sqrt{0.21N}}\right)\geq 0.997$

$\Rightarrow$ $\varPhi\left(\frac{80-0.7N}{\sqrt{0.21N}}\right)\leq 0.003$ $\Rightarrow$ $\frac{80-0.7N}{\sqrt{0.21N}}\leq -2.75$

## 4\*. 伯努利大数定律
$n$次独立重复试验中，事件A发生的次数$m$，$P(A)=p$，则对任意$\epsilon>0$，有$\lim\limits_{n\rightarrow\infty}P\left(\left|\frac{m}{n}-p\right|\leq \epsilon\right)=1$

## 5\*. 辛钦大数定律
$\mathbb{X}=(X_1,X_2,\cdots,X_n)$独立同分布，记$X_i=\begin{cases}1&,A发生 \\\\ 0&,A不发生\end{cases}$，则对任意$\epsilon>0$，有$\lim\limits_{n\rightarrow\infty}P\left(\left|\frac{\sum X_i}{n}-p\right|\leq \epsilon\right)=1$

## 6\*. 切比雪夫大数定律
$\mathbb{X}=(X_1,X_2,\cdots,X_n)$不相关，$\exists E(X_i),D(X_i)<\infty$，则对任意$\epsilon>0$，有$\lim\limits_{n\rightarrow\infty}P\left(\left|\frac{\sum X_i}{n}-\frac{\sum E(X_i)}{n}\right|\leq \epsilon\right)=1$

# 六、抽样分布
## 1. 统计量
1. 样本均值：$\overline{X}=\frac{1}{n}\sum X_i$
2. 样本方差：$S^2=\frac{1}{n-1}\sum(X_i-\overline{X})^2$
3. 样本标准差：$S=\sqrt{S^2}$
4. 样本$k$阶原点矩：$A_k=\frac{1}{n}\sum X_i^k$
5. 样本$k$阶中心矩：$B_k=\frac{1}{n}\sum(X_i-\overline{X})^k$

$\overline{X}=A_1$，$S^2=\frac{n-1}{n}B_2$，$B_1=0$

## 2. $X$~$N(\mu,\sigma^2)$
1. 分布名称：正态分布
2. 上$\alpha$分位数：$u_\alpha$

## 3. $X$~$\chi^2(n)$
1. 分布名称：卡方分布
2. 概述：$X_1,X_2,\cdots,X_n$独立同分布，且$X_i$ ~ $N(0,1)$，则$X=\sum X_i^2$ ~ $\chi^2(n)$
3. 上$\alpha$分位数：$\chi_\alpha^2(n)$
4. 期望：$E(X)=n$
5. 方差：$D(X)=2n$
6. $n\rightarrow\infty$时，$\chi^2(n)$近似于$N(n,2n)$
7. 可加性：$X_i$ ~ $\chi^2(n_i)$ $\Rightarrow$ $\sum X_i$ ~ $\chi^2(\sum n_i)$

## 4. $X$~$t(n)$
1. 分布名称：t分布
2. 概述：$X$ ~ $N(0,1)$，$Y$ ~ $\chi^2(n)$，$X,Y$独立，$\frac{X}{\sqrt{\frac{Y}{n}}}$ ~ $t(n)$
3. 上$\alpha$分位数：$t_\alpha(n)$
4. 期望：$E(X)=0$ 对称性：$t_{1-\alpha}(n)=-t_\alpha(n)$
5. $n\rightarrow\infty$时，$t(n)$近似于$N(0,1)$

## 5. $X$~$F(n_1,n_2)$
1. 分布名称：F分布
2. 概述：$X_1$ ~ $\chi^2(n_1)$，$X_2$ ~ $\chi^2(n_2)$，$X_1,X_2$独立，$\frac{X_1/n_1}{X_2/n_2}$ ~ $F(n_1,n_2)$
3. 上$\alpha$分位数：$F_\alpha(n_1,n_2)$
4. 性质1：$F_{1-\alpha}(n_1,n_2)=\frac{1}{F_\alpha(n_2,n_1)}$
5. 性质2：$P(F\leq F_\alpha(n_1,n_2))=1-\alpha$

## 6.单正态总体下的抽样分布
1. $\overline{X}$ ~ $N(\mu,\frac{\sigma^2}{n})$
2. $U=\frac{\overline{X}-\mu}{\sigma}{\sqrt{n}}$ ~ $N(0,1)$
3. $T=\frac{\overline{X}-\mu}{S}{\sqrt{n}}$ ~ $t(n-1)$
4. $C=\frac{(n-1)S^2}{\sigma^2}=\frac{1}{\sigma^2}\sum(X_i-\overline{X})^2$ ~ $\chi^2(n-1)$

## 7.两正态总体下的抽样分布
1. $\overline{X}-\overline{Y}$ ~ $N(\mu_1-\mu_2,\frac{\sigma_1^2}{n_1}+\frac{\sigma_2^2}{n_2})$ $\Rightarrow$ $U=\frac{(\overline{X}-\overline{Y})-(\mu_1-\mu_2)}{\sqrt{\frac{\sigma_1^2}{n_1}+\frac{\sigma_2^2}{n_2}}}$ ~ $N(0,1)$
2. $\frac{(\overline{X}-\overline{Y})-(\mu_1-\mu_2)}{S_w\sqrt{\frac{1}{n_1}+\frac{1}{n_2}}}$ ~ $t(n_1+n_2-2)$ $\Rightarrow$ $T=\frac{(\overline{X}-\overline{Y})-(\mu_1-\mu_2)}{S_w\sqrt{\frac{1}{n_1}+\frac{1}{n_2}}}$ ~ $t(n_1+n_2-2)$
3. $\frac{S_1^2/\sigma_1^2}{S_2^2/\sigma_2^2}$ ~ $F(n_1-1,n_2-1)$ $\Rightarrow$ $F=\frac{S_1^2/\sigma_1^2}{S_2^2/\sigma_2^2}$ ~ $F(n_1-1,n_2-1)$

注：$S_w^2=\frac{(n_1-1)S_1^2+(n_2-1)S_2^2}{n_1+n_2-2}$

# 七、参数估计（点估计）
## 1. 矩估计
利用样本矩去估计总体矩，建立样本矩与总体矩的关系，解出参数

$A_1=\overline{X}=E(X)$，$A_2=\frac{1}{n}\sum X_i^2=E(X^2)$，$B_2=A_2-\overline{X}^2$

$\Rightarrow$ $\mu=A_1$，$\sigma^2=B_2$

## 2. 极大似然估计
1. 写出总体的密度函数$f(x;\theta)$
2. 写出样本的似然函数$L(\theta)=\prod f(x_i;\theta)$
3. 对$L(\theta)$取对数，得到对数似然函数$\ln L(\theta)$
4. 对$\ln L(\theta)$求导，令其等于0，解出$\theta$的值
5. 多个参数时，对每个参数分别求偏导，令其等于0，得到方程组，解出每个参数的值

## 3. 点估计的优良性
1. 无偏性：$E(\hat{\theta})=\theta$
2. 有效性：$D(\hat{\theta})\leq D(\tilde{\theta})$，$\hat{\theta}=\overline{X}$时最有效

### 例题：设$X_1,X_2,\cdots,X_n$是来自总体$X$的样本，$X$的密度函数为$f(x;\sigma)=\frac{1}{2\sigma}e^{-\frac{|x|}{\sigma}}$，其中$\sigma>0$为未知参数，求$\sigma$的矩估计和极大似然估计。
矩估计

$E(X)=\int_{-\infty}^{+\infty}xf(x;\sigma)dx=\frac{1}{2\sigma}\int_{-\infty}^{+\infty}xe^{-\frac{|x|}{\sigma}}dx=0$，无法求出$\sigma$，故用二阶矩求解

$E(X^2)=\int_{-\infty}^{+\infty}x^2f(x;\sigma)dx=\frac{1}{2\sigma}\int_{-\infty}^{+\infty}x^2e^{-\frac{|x|}{\sigma}}dx=2\sigma^2$

$A_2=2\hat\sigma^2$，解得$\hat\sigma=\sqrt{\frac{A_2}{2}}$

极大似然估计

$L(\hat\sigma)=\prod f(x_i;\hat\sigma)=\prod\frac{1}{2\hat\sigma}e^{-\frac{|x_i|}{\hat\sigma}}=\frac{1}{2^n\hat\sigma^n}e^{-\frac{\sum|x_i|}{\hat\sigma}}$

$\ln L(\hat\sigma)=-n\ln(2\hat\sigma)-\frac{\sum|x_i|}{\hat\sigma}$

$\frac{d\ln L(\hat\sigma)}{d\hat\sigma}=-\frac{n}{\hat\sigma}+\frac{\sum|x_i|}{\hat\sigma^2}=0$，解得$\hat\sigma=\frac{\sum|x_i|}{n}$

# 八、假设检验
## 1. 基本步骤
1. 建立原假设$H_0$和备择假设$H_1$，确定显著性水平$\alpha$
2. 选取检验统计量$U$，此时拒绝域$C=\{|U| \ge u_\frac{\alpha}{2}\}$
3. 计算检验统计量的值$u$，若$u\in C$，则拒绝$H_0$，否则接受$H_0$

## 2. 单个正态总体的假设检验
### $H_0:\mu=\mu_0$，$H_1:\mu\neq\mu_0$
1. $\sigma^2$已知，$U=\frac{\overline{X}-\mu_0}{\sigma/\sqrt{n}}$ ~ $N(0,1)$
2. $\sigma^2$未知，$T=\frac{\overline{X}-\mu_0}{S/\sqrt{n}}$ ~ $t(n-1)$
### $H_0:\sigma^2=\sigma_0^2$，$H_1:\sigma^2\neq\sigma_0^2$
$\chi^2=\frac{(n-1)S^2}{\sigma_0^2}$ ~ $\chi^2(n-1)$

## 3. 两个正态总体的假设检验
### $H_0:\mu_1=\mu_2$，$H_1:\mu_1\neq\mu_2$
1. $\sigma_1^2,\sigma_2^2$已知，$U=\frac{(\overline{X}-\overline{Y})-(\mu_1-\mu_2)}{\sqrt{\frac{\sigma_1^2}{n_1}+\frac{\sigma_2^2}{n_2}}}$ ~ $N(0,1)$
2. $\sigma_1^2,\sigma_2^2$未知，$T=\frac{(\overline{X}-\overline{Y})-(\mu_1-\mu_2)}{S_w\sqrt{\frac{1}{n_1}+\frac{1}{n_2}}}$ ~ $t(n_1+n_2-2)$
### $H_0:\sigma_1^2=\sigma_2^2$，$H_1:\sigma_1^2\neq\sigma_2^2$
$F=\frac{S_1^2}{S_2^2}$ ~ $F(n_1-1,n_2-1)$