# 1_Lagrange Interpolating Polynomials
## Weierstrass Approximation Theorem
**Algebraic Polynomials**
functions mapping the set of real numbers into itself
$$
P_n(x)=a_n x^n+a_{n-1} x^{n-1}+\cdots+a_1 x+a_0
$$
where $n$ is a nonnegative integer and $a_0, \ldots, a_n$ are real constants.

**Weierstrass Approximation Theorem**
Suppose that $f$ is defined and continuous on $[a, b]$. For each $\epsilon>0$, there exists a polynomial $P(x)$, with the property that
$$
|f(x)-P(x)|<\epsilon, \qquad \text{for all} \; x\; \text{in} \; [a, b]
$$

## Inaccuracy of Taylor Polynomials
- For the Taylor polynomials, all the information used in the approximation is concentrated at the single number $x_0$, so these polynomials will generally give inaccurate approximations as we move away from $x_0$.
- For ordinary computational purposes, it is more efficient to use methods that include information at various points.
- **The primary use** of Taylor polynomials in numerical analysis is not for approximation purposes, but for the derivation of numerical techniques and error estimation.

## Lagrange Polynomial
We first construct, for each $k=0,1, \ldots, n$, a function $L_{n, k}(x)$ with the property that $L_{n, k}\left(x_i\right)=0$ when $i \neq k$ and $L_{n, k}\left(x_k\right)=1$. So 
$$
\begin{aligned}
L_{n, k}(x) & =\frac{\left(x-x_0\right)\left(x-x_1\right) \cdots\left(x-x_{k-1}\right)\left(x-x_{k+1}\right) \cdots\left(x-x_n\right)}{\left(x_k-x_0\right)\left(x_k-x_1\right) \cdots\left(x_k-x_{k-1}\right)\left(x_k-x_{k+1}\right) \cdots\left(x_k-x_n\right)} \\
& =\prod_{\substack{i=0 \\
i \neq k}}^n \frac{\left(x-x_i\right)}{\left(x_k-x_i\right)}
\end{aligned}
$$
![image.png](https://image-host-1316314535.cos.ap-beijing.myqcloud.com/Obsidian%20Image/The%20Lagrange%20Polynomial.png)

The n-th Lagrange interpolating polynomial
$$
P(x)=f\left(x_0\right) L_{n, 0}(x)+\cdots+f\left(x_n\right) L_{n, n}(x)=\sum_{k=0}^n f\left(x_k\right) L_{n, k}(x)
$$
We will write $L_{n, k}(x)$ simply as $L_k(x)$ when there is no confusion as to its degree.

# 2_Interpolating Polynomial Error Bound
## Theorem
Suppose $x_0, x_1, \ldots, x_n$ are distinct numbers in the interval $[a, b]$ and $f \in C^{n+1}[a, b]$. Then, for each $x$ in $[a, b]$, a number $\xi(x)$ (generally unknown) between $x_0, x_1, \ldots, x_n$, and hence in $(a, b)$, exists with
$$
f(x)=P(x)+\frac{f^{(n+1)}(\xi(x))}{(n+1) !}\left(x-x_0\right)\left(x-x_1\right) \cdots\left(x-x_n\right)
$$
where $P(x)$ is the interpolating polynomial given by
$$
P(x)=f\left(x_0\right) L_{n, 0}(x)+\cdots+f\left(x_n\right) L_{n, n}(x)=\sum_{k=0}^n f\left(x_k\right) L_{n, k}(x)
$$

**Proof**
1. 对于 $x=x_k$，下式显然成立
$$
f(x)=P(x)+\frac{f^{(n+1)}(\xi(x))}{(n+1) !}\left(x-x_0\right)\left(x-x_1\right) \cdots\left(x-x_n\right)
$$
2. 对于 $x \neq x_k$，证明方法如下：对于 $t \in [a,b]$，定义函数 $g$ 
$$
\begin{aligned}
g(t) & =f(t)-P(t)-[f(x)-P(x)] \frac{\left(t-x_0\right)\left(t-x_1\right) \cdots\left(t-x_n\right)}{\left(x-x_0\right)\left(x-x_1\right) \cdots\left(x-x_n\right)} \\
& =f(t)-P(t)-[f(x)-P(x)] \prod_{i=0}^n \frac{\left(t-x_i\right)}{\left(x-x_i\right)}
\end{aligned}
$$
- 其中 $g\left(x_k\right)=0$，$g(x)=0$，证明如下：
	- Since $f \in C^{n+1}[a, b]$, and $P \in C^{\infty}[a, b]$, it follows that $g \in C^{n+1}[a, b]$. For $t=x_k$, we have
$$
g\left(x_k\right)=f\left(x_k\right)-P\left(x_k\right)-[f(x)-P(x)] \prod_{i=0}^n \frac{\left(x_k-x_i\right)}{\left(x-x_i\right)}=0-[f(x)-P(x)] \cdot 0=0
$$
- We have seen that Furthermore, 
$$
\begin{aligned}
g(x) & =f(x)-P(x)-[f(x)-P(x)] \prod_{i=0}^n \frac{\left(x-x_i\right)}{\left(x-x_i\right)} \\
& =f(x)-P(x)-[f(x)-P(x)]=0
\end{aligned}
$$
- 所以 $g \in C^{n+1}[a, b]$, 且 $g$ 在 $n+2$ 个不同点 $x, x_0, x_1, \ldots, x_n$ 上值等于 0
- 通过广义罗尔定理（Generalized Rolle's Theorem），存在 $\xi \in (a, b)$ 满足下式
$$
\begin{aligned}
0 & =g^{(n+1)}(\xi) \\
& =f^{(n+1)}(\xi)-P^{(n+1)}(\xi)-[f(x)-P(x)] \frac{d^{n+1}}{d t^{n+1}}\left[\prod_{i=0}^n \frac{\left(t-x_i\right)}{\left(x-x_i\right)}\right]_{t=\xi}
\end{aligned}
$$
- 因为 $P(x)$ 最多是 $n$ 阶多项式，所以 $P^{(n+1)}(x)=0$.
	- 又因为 $\prod_{i=0}^n \frac{t-x_i}{x-x_i}$ 是 $n+1$ 阶多项式，所以
$$
\prod_{i=0}^n \frac{\left(t-x_i\right)}{\left(x-x_i\right)}=\left[\frac{1}{\prod_{i=0}^n\left(x-x_i\right)}\right] t^{n+1}+(\text { lower-degree terms in } t)
$$
且
$$
\frac{d^{n+1}}{d t^{n+1}} \prod_{i=0}^n \frac{\left(t-x_i\right)}{\left(x-x_i\right)}=\frac{(n+1) !}{\prod_{i=0}^n\left(x-x_i\right)}
$$
- 综上 
$$
\begin{aligned}
0 & =f^{(n+1)}(\xi)-P^{(n+1)}(\xi)-[f(x)-P(x)] \frac{d^{n+1}}{d t^{n+1}}\left[\prod_{i=0}^n \frac{\left(t-x_i\right)}{\left(x-x_i\right)}\right]_{t=\xi} \\
& =f^{(n+1)}(\xi)-0-[f(x)-P(x)] \frac{(n+1) !}{\prod_{i=0}^n\left(x-x_i\right)}
\end{aligned}
$$ 
通过求解 $f(x)$ 可以得到所需表达式
$$
f(x)=P(x)+\frac{f^{(n+1)}(\xi)}{(n+1) !} \prod_{i=0}^n\left(x-x_i\right)
$$

### Generalized Rolle's Theorem
Suppose $f \in C[a, b]$ is $n$ times differentiable on $(a, b)$. If
$$
f(x)=0
$$
at the $n+1$ distinct numbers $a \leq x_0<x_1<\ldots<x_n \leq b$, then a number $c$ in $\left(x_0, x_n\right)$, and hence in $(a, b)$, exists with
$$
f^{(n)}(c)=0
$$
## Example: Tabulated Data
**Question**
- Suppose that a table is to be prepared for the function $f(x)=e^x$, for $x$ in $[0,1]$.
- Assume that the number of decimal places to be given per entry is $d \geq 8$ and that the difference between adjacent $x$ -values, the step size, is $h$.
- What step size $h$ will ensure that linear interpolation gives an absolute error of at most $10^{-6}$ for all $x$ in $[0,1]$ ?

**Answer**
Let $x_0, x_1, \ldots$ be the numbers at which $f$ is evaluated, $x$ be in $[0,1]$, and suppose $j$ satisfies $x_j \leq x \leq x_{j+1}$. The error bound theorem implies that the error in linear interpolation is
$$
|f(x)-P(x)|=\left|\frac{f^{(2)}(\xi)}{2 !}\left(x-x_j\right)\left(x-x_{j+1}\right)\right|=\frac{\left|f^{(2)}(\xi)\right|}{2}\left|\left(x-x_j\right)\right|\left|\left(x-x_{j+1}\right)\right|
$$
The step size is $h$, so $x_j=j h, x_{j+1}=(j+1) h$, and
$$
|f(x)-P(x)| \leq \frac{\left|f^{(2)}(\xi)\right|}{2 !}|(x-j h)(x-(j+1) h)| .
$$
Hence
$$
\begin{aligned}
|f(x)-P(x)| & \leq \frac{\max _{\xi \in[0,1]} e^{\xi}}{2} \max _{x_j \leq x \leq x_{j+1}}|(x-j h)(x-(j+1) h)| \\
& \leq \frac{e}{2} \max _{x_j \leq x \leq x_{j+1}}|(x-j h)(x-(j+1) h)|
\end{aligned}
$$
Consider the function $g(x)=(x-j h)(x-(j+1) h)$, for $j h \leq x \leq(j+1) h$. Because
$$
g^{\prime}(x)=(x-(j+1) h)+(x-j h)=2\left(x-j h-\frac{h}{2}\right)
$$
the only critical point for $g$ is at $x=j h+\frac{h}{2}$, with
$$
g\left(j h+\frac{h}{2}\right)=\left(\frac{h}{2}\right)^2=\frac{h^2}{4}
$$

Since $g(j h)=0$ and $g((j+1) h)=0$, the maximum value of $\left|g^{\prime}(x)\right|$ in $[j h,(j+1) h]$ must occur at the critical point.
This implies that
$$
|f(x)-P(x)| \leq \frac{e}{2} \max _{x_j \leq x \leq x_{j+1}}|g(x)| \leq \frac{e}{2} \cdot \frac{h^2}{4}=\frac{e h^2}{8} 
$$
Consequently, to ensure that the the error in linear interpolation is bounded by $10^{-6}$, it is sufficient for $h$ to be chosen so that
$$
\frac{e h^2}{8} \leq 10^{-6} 
$$
This implies that $h<1.72 \times 10^{-3}$
Because $n=\frac{(1-0)}{h}$ must be an integer, a reasonable choice for the step size is $h=0.001$.

# 3_Divided Differences
- The divided differences of $f$ with respect to $x_0, x_1, \ldots, x_n$ are used to express $P_n(x)$ in the form
$$
P_n(x)=a_0+a_1\left(x-x_0\right)+a_2\left(x-x_0\right)\left(x-x_1\right)+\cdots+a_n\left(x-x_0\right) \cdots\left(x-x_{n-1}\right)
$$
for appropriate constants $a_0, a_1, \ldots, a_n$.

## The Divided Difference Notation
We now introduce the divided-difference notation, which is related to Aitken's $\Delta^2$ notation
**Forward Difference Operator $\Delta$**
- For a given sequence $\left\{p_n\right\}_{n=0}^{\infty}$, the forward difference $\Delta p_n$ (read "delta $p_n$") is defined by
$$
\Delta p_n=p_{n+1}-p_n, \quad \text { for } n \geq 0 .
$$
- Higher powers of the operator $\Delta$ are defined recursively by
$$
\Delta^k p_n=\Delta\left(\Delta^{k-1} p_n\right), \quad \text { for } k \geq 2
$$
So
- the $k$ th divided difference relative to $x_i, x_{i+1}, x_{i+2}, \ldots, x_{i+k}$ is
$$
f\left[x_i, x_{i+1}, \ldots, x_{i+k-1}, x_{i+k}\right] =\frac{f\left[x_{i+1}, x_{i+2}, \ldots, x_{i+k}\right]-f\left[x_i, x_{i+1}, \ldots, x_{i+k-1}\right]}{x_{i+k}-x_i}
$$
- The process ends with the single $n$ th divided difference,
$$
f\left[x_0, x_1, \ldots, x_n\right]=\frac{f\left[x_1, x_2, \ldots, x_n\right]-f\left[x_0, x_1, \ldots, x_{n-1}\right]}{x_n-x_0}
$$
### Generating the Divided Difference Table
$$
\begin{array}{ccccc}
\hline x & f(x) & \text { divided differences } & \begin{array}{c}
\text { Second } \\
\text { divided differences }
\end{array} & \begin{array}{c}
\text { Third } \\
\text { divided differences }
\end{array} \\
\hline x_0 & f\left[x_0\right] & & \\
& & f\left[x_0, x_1\right]=\frac{f\left[x_1\right]-f\left[x_0\right]}{x_1-x_0} & & \\
x_1 & f\left[x_1\right] & & f\left[x_0, x_1, x_2\right]=\frac{f\left[x_1, x_2\right]-f\left[x_0, x_1\right]}{x_2-x_0} & \\
& & f\left[x_1, x_2\right]=\frac{f\left[x_2\right]-f\left[x_1\right]}{x_2-x_1} & & f\left[x_0, x_1, x_2, x_3\right]=\frac{f\left[x_1, x_2, x_3\right]-f\left[x_0, x_1, x_2\right]}{x_3-x_0} \\
x_2 & f\left[x_2\right] & & f\left[x_1, x_2, x_3\right]=\frac{f\left[x_2, x_3\right]-f\left[x_1, x_2\right]}{x_3-x_1} & \\
& & f\left[x_2, x_3\right]=\frac{f\left[x_3\right]-f\left[x_2\right]}{x_3-x_2} & &f\left[x_1, x_2, x_3, x_4\right]=\frac{f\left[x_2, x_3, x_4\right]-f\left[x_1, x_2, x_3\right]}{x_4-x_1} \\
x_3 & f\left[x_3\right] & & f\left[x_2, x_3, x_4\right]=\frac{f\left[x_3, x_4\right]-f\left[x_2, x_3\right]}{x_4-x_2} \\
& & f\left[x_3, x_4\right]=\frac{f\left[x_4\right]-f\left[x_3\right]}{x_4-x_3} & &f\left[x_2, x_3, x_4, x_5\right]=\frac{f\left[x_3, x_4, x_5\right]-f\left[x_2, x_3, x_4\right]}{x_5-x_2} \\
x_4 & f\left[x_4\right] & & f\left[x_3, x_4, x_5\right]=\frac{f\left[x_4, x_5\right]-f\left[x_3, x_4\right]}{x_5-x_3} & \\
& & f\left[x_4, x_5\right]=\frac{f\left[x_5\right]-f\left[x_4\right]}{x_5-x_4} & & \\
x_5 & f\left[x_5\right] & & \\
\hline
\end{array}
$$

## Newton's Divided Difference Interpolating Polynomial
$$
P_n(x)=a_0+a_1\left(x-x_0\right)+a_2\left(x-x_0\right)\left(x-x_1\right)+\cdots+a_n\left(x-x_0\right) \cdots\left(x-x_{n-1}\right)
$$
As might be expected from the evaluation of $a_0$ and $a_1$, the required constants are
$$
a_k=f\left[x_0, x_1, x_2, \ldots, x_k\right]
$$
for each $k=0,1, \ldots, n$.
So $P_n(x)$ can be rewritten in a form called Newton's Divided-Difference:
$$
P_n(x)=f\left[x_0\right]+\sum_{k=1}^n f\left[x_0, x_1, \ldots, x_k\right]\left(x-x_0\right) \cdots\left(x-x_{k-1}\right)
$$