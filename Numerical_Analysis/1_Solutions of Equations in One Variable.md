# 1_The Bisection Method
## The Root-Finding Problem
find a root or solution of an equation of the form 
$$
f(x)=0
$$
for a given function f.

## Bisection Method
first consider the Bisection (Binary search) Method which is based on the Intermediate Value Theorem.
![image.png](https://image-host-1316314535.cos.ap-beijing.myqcloud.com/Obsidian%20Image/TheBisectionMethod.png)

### Intermediate Value Theorem (VIT) 
Consider an arbitray function $f(x)$ on $[a,b]$. We are given a number K such that $K \in [f(a),f(b)]$. If $f \in C[a,b]$ and K is any number between $f(a)$ and $f(b)$, then there exists a number $c \in (a,b)$ for which $f(c)=K$.

> $C[a,b]$ 表示在 $[a,b]$ 上连续的全体函数组成的集合，其中 $C$ 是英文 Continuous 的首字母缩写。$f \in C[a,b]$ 的意思：是 $f(x)$ 是一个在 $[a,b]$ 上连续的函数。
> $f \in C^n[a, b] \to f^{n} \in C[a, b]$。例如 $f \in C^1[a,b]$ 表示的是函数 $f(x)$ 的一阶导数在区间 $[a,b]$ 上是连续的函数。

### Algorithm
Given the function $f$ defined on $[a,b]$ satisfying $f(a)f(b)<0$
The Bisection Method to solve $f(x)=0$:
1. $a_1=a, b_1=b, p_0=a$;
2. $i=1$;
3. $p_i=\frac{1}{2}\left(a_i+b_i\right)$;
4. If $\left|p_i-p_{i-1}\right|<\epsilon$ or $\left|f\left(p_i\right)\right|<\epsilon$ then 10;
5. If $f\left(p_i\right) f\left(a_i\right)>0$, then 6 ;
If $f\left(p_i\right) f\left(a_i\right)<0$, then 8 ;
6. $a_{i+1}=p_i, b_{i+1}=b_i$;
7. $i=i+1$; go to 3 ;
8. $a_{i+1}=a_i ; b_{i+1}=p_i$;
9. $i=i+1$; go to 3 ;
10. End of Procedure.

## Theoretical Result for the Bisection Method
Suppose that $f \in C[a, b]$ and $f(a) \cdot f(b)<0$. The Bisection method yenerates a sequence $\left\{p_n\right\}_{n=1}^{\infty}$ approximating a zero $p$ of $f$ with
$$
\left|p_n-p\right| \leq \frac{b-a}{2^n}, \quad \text { when } \quad n \geq 1
$$

## Summary
- Firstly it is very slow to converge in that $N$ may become quite large before $p-p_N$ becomes sufficiently small.
- Also it is possible that a good intermediate approximation may be inadvertently discarded.
- It will always converge to a solution however and, for this reason, is often used to provide a good initial approximation for a more efficient procedure.

# 2_Fixed-Point Iteration
## The Fixed Point Problem 
Given a function $f(x)$ where $a\leq x\leq b$, find values $p$ such that 
$$
f (p)=0
$$
Then construct an auxiliary function $g(x)$ such that 
$$
p=g(p)
$$
whenever $f(p)=0$
The problem of finding $p$ such that $p=g(p)$ is known as **the fixed point problem**.

### Existence of a Fixed Point
If $g \in C[a,b]$ and $g (x)\in [a,b]$ for all $x\in [a,b]$ then the function $g$ has a fixed point in $[a,b]$.

**Proof**
- If $g(a)=a$ or $g(b)=b$, the existence of a fixed point is obvious. 
- Suppose not; then it must be true that $g(a)>a$ and $g(b)<b$. Define $h(x)=g(x)-x ; h$ is continuous on $[a, b]$ and, moreover, 
$$
h(a)=g(a)-a>0, \quad h(b)=g(b)-b<0 
$$
- The Intermediate Value Theorem implies that there exists $p \in(a, b)$ for which $h(p)=0$.
- Thus $g (p)-p=0$ and $p$ is a fixed point of $g$ .

## Uniqueness Result 
In order to ensure that $g(x)$ has a unique fixed point in interval $I=[a,b]$, we must make an additional assumption that $g(x)$ does not vary too rapidly.
**Fixed-Point Theorem**
Let $g\in C[a,b]$ and $g (x)\in [a,b]$ for all $x\in[a,b]$. Further if $g^\prime(x)$ exists on $(a,b)$ and
$$\lvert g^\prime (x) \rvert\leq k<1,\forall x\in [a,b]$$
then the function $g$ has a unique fixed point p in $[a,b]$

**Proof**
- Assuming the hypothesis of the theorem, suppose that $p$ and $q$ are both fixed points in $[a, b]$ with $p \neq q$.
- By the Mean Value Theorem, a number $\xi$ exists between $p$ and $q$ and hence in $[a, b]$ with
$$
\begin{aligned}
|p-q|=|g(p)-g(q)| & =\left|g^{\prime}(\xi)\right||p-q| \\
& \leq k|p-q| \\
& <|p-q|
\end{aligned}
$$
which is a contradiction.
- This contradiction must come from the only supposition, $p \neq q$. Hence, $p=q$ and the fixed point in $[a, b]$ is unique.

### Mean Value Theorem
If $f\in C[a,b]$ and $f$ is differentiable on $(a,b)$, then a number $c$ exists such that 
$$
f^\prime(c)=\frac{f(b)-f(a)}{b-a}
$$

## Functional (Fixed-Point) Iteration
- To approximate the fixed point of a function $g$, we choose an initial approximation $p_0$ and generate the sequence $\left\{p_n\right\}_{n=0}^{\infty}$ by letting $p_n=g\left(p_{n-1}\right)$, for each $n \geq 1$.
- If the sequence converges to $p$ and $g$ is continuous, then
$$
p=\lim _{n \rightarrow \infty} p_n=\lim _{n \rightarrow \infty} g\left(p_{n-1}\right)=g\left(\lim _{n \rightarrow \infty} p_{n-1}\right)=g(p),
$$
and a solution to $x=g(x)$ is obtained.
- This technique is called fixed-point, or functional iteration.

### Algorithm
To find the fixed point of $g$ in an interval $[a, b]$, given the equation $x=g(x)$ with an initial guess $p_0 \in[a, b]$ :
1. $n=1$;
2. $p_n=g\left(p_{n-1}\right)$;
3. If $\left|p_n-p_{n-1}\right|<\epsilon$ then 5 ;
4. $n \rightarrow n+1$; go to 2 .
5. End of Procedure.

## Convergence Criteria for the Fixed-Point Method
The following theorem and its corollary give us some clues concerning the paths we should pursue and, perhaps more importantly, **some we should reject**.

### Convergence Result
Let $g \in C[a, b]$ with $g(x) \in[a, b]$ for all $x \in[a, b]$. Let $g^{\prime}(x)$ exist on $(a, b)$ with
$$
\left|g^{\prime}(x)\right| \leq k<1, \quad \forall x \in[a, b] .
$$
If $p_0$ is any point in $[a, b]$ then the sequence defined by
$$
p_n=g\left(p_{n-1}\right), \quad n \geq 1,
$$
will converge to the unique fixed point $p$ in $[a, b]$.

**Proof**
- By the Uniquenes Theorem, a unique fixed point exists in $[a, b]$.
- Using the Mean Value Theorem and the assumption that $\left|g^{\prime}(x)\right| \leq k<1, \forall x \in[a, b]$, we write
$$
\begin{aligned}
\left|p_n-p\right| & =\left|g\left(p_{n-1}\right)-g(p)\right| \\
& \leq\left|g^{\prime}(\xi)\right|\left|p_{n-1}-p\right| \\
& \leq k\left|p_{n-1}-p\right|
\end{aligned}
$$
where $\xi \in(a, b)$.
- Applying the inequality of the hypothesis inductively gives
$$
\begin{aligned}
\left|p_n-p\right| & \leq k\left|p_{n-1}-p\right| \\
& \leq k^2\left|p_{n-2}-p\right| \\
& \leq k^n\left|p_0-p\right|
\end{aligned}
$$
- Since $k<1$, 
$$
\lim _{n \rightarrow \infty}\left|p_n-p\right| \leq \lim _{n \rightarrow \infty} k^n\left|p_0-p\right|=0,
$$
and $\left\{p_n\right\}_{n=0}^{\infty}$ converges to $p$.

### Corrollary to the Convergence Result
If $g$ satisfies the hypothesis of the Theorem, then 
$$
\left|p_n-p\right| \leq \frac{k^n}{1-k}\left|p_1-p_0\right|
$$
It is important to realise that the estimate for the number of iterations required given by the theorem is an upper bound. 
The smaller the bound on the magnitude of $\left|g^{\prime}(x)\right|$ is, the much faster the speed of convergence is.

**Proof**
- For $n \geq 1$, the procedure used in the proof of the theorem implies that
$$
\begin{aligned}
\left|p_{n+1}-p_n\right| & =\left|g\left(p_n\right)-g\left(p_{n-1}\right)\right| \\
& \leq k\left|p_n-p_{n-1}\right| \\
& \leq \cdots \\
& \leq k^n\left|p_1-p_0\right|
\end{aligned}
$$
- Thus, for $m>n \geq 1$,
$$
\begin{aligned}
\left|p_m-p_n\right| & =\left|p_m-p_{m-1}+p_{m-1}-p_{m-2}+\cdots+p_{n+1}-p_n\right| \\
& \leq\left|p_m-p_{m-1}\right|+\left|p_{m-1}-p_{m-2}\right|+\cdots+\left|p_{n+1}-p_n\right| \\
& \leq k^{m-1}\left|p_1-p_0\right|+k^{m-2}\left|p_1-p_0\right|+\cdots+k^n\left|p_1-p_0\right| \\
& \leq k^n\left(1+k+k^2+\cdots+k^{m-n-1}\right)\left|p_1-p_0\right| .
\end{aligned}
$$
- However, since $\lim _{m \rightarrow \infty} p_m=p$, we obtain
$$
\begin{aligned}
\left|p-p_n\right| & =\lim _{m \rightarrow \infty}\left|p_m-p_n\right| \\
& \leq k^n\left|p_1-p_0\right| \sum_{i=0}^{\infty} k^i \\
& =\frac{k^n}{1-k}\left|p_1-p_0\right| .
\end{aligned}
$$

# 3_Newton's Method
## Newton's (or the Newton-Raphson) Method
### Derivation
- Suppose that $f \in C^2[a, b]$. Let $p_0 \in[a, b]$ be an approximation to $p$ such that $f^{\prime}\left(p_0\right) \neq 0$ and $\left|p-p_0\right|$ is "small."
- Consider the first Taylor polynomial for $f(x)$ expanded about $p_0$ and evaluated at $x=p$.
$$
f(p)=f\left(p_0\right)+\left(p-p_0\right) f^{\prime}\left(p_0\right)+\frac{\left(p-p_0\right)^2}{2} f^{\prime \prime}(\xi(p)),
$$
where $\xi(p)$ lies between $p$ and $p_0$.
- Since $f(p)=0$, this equation gives
$$
0=f\left(p_0\right)+\left(p-p_0\right) f^{\prime}\left(p_0\right)+\frac{\left(p-p_0\right)^2}{2} f^{\prime \prime}(\xi(p))
$$
- Newton's method is derived by assuming that since $\left|p-p_0\right|$ is small, the term involving $\left(p-p_0\right)^2$ is much smaller, so
$$
0 \approx f\left(p_0\right)+\left(p-p_0\right) f^{\prime}\left(p_0\right)
$$
- Solving for $p$ gives 
$$
p \approx p_0-\frac{f\left(p_0\right)}{f^{\prime}\left(p_0\right)} \equiv p_1 
$$

**Summary**
This sets the stage for Newton's method, which starts with an initial approximation $p_{0}$ and generates the sequence $\left\{p_n\right\}_{n=0}^{\infty}$, by
$$
p_{n}=p_{n-1}-\frac{f(p_{n-1})}{f^{\prime}(p_{n-1})} \qquad for \; n\geq 1
$$

### Newton's Method as a functional Iteration Technique
because $p_{n}=g(p_{n-1})$
so 
$$
g(p_{n-1})=p_{n-1}-\frac{f(p_{n-1})}{f^{\prime}(p_{n-1})}
$$

### Algorithm
To find a solution to $f(x)=0$ given an initial approximation $p_0$ :
1. Set $i=0$;
2. While $i \leq N$, do Step 3:
3. step3
	1. If $f^{\prime}\left(p_0\right)=0$ then Step 5 .
	2. Set $p=p_0-f\left(p_0\right) / f^{\prime}\left(p_0\right)$;
	3. If $\left|p-p_0\right|<$ TOL then Step 6;
	4. Set $i=i+1$;
	5. Set $p_0=p$;
4. Output a 'failure to converge within the specified number of iterations' message \& Stop;
5. Output an appropriate failure message (zero derivative) \& Stop;
6. Output $p$

### Stopping Criteria for the Algorithm
- Various stopping procedures can be applied in Step 3.3.
- We can select a tolerance $\epsilon>0$ and generate $p_1, \ldots, p_N$ until one of the following conditions is met:
$$
\begin{aligned}
\left|p_N-p_{N-1}\right| & <\epsilon \\
\frac{\left|p_N-p_{N-1}\right|}{\left|p_N\right|} & <\epsilon, \quad p_N \neq 0, \quad \text { or } \\
\left|f\left(p_N\right)\right| & <\epsilon
\end{aligned}
$$
- Note that none of these inequalities give precise information about the actual error $\left|p_N-p\right|$.

## Convergence Theorem for Newton's Method
Let $f \in C^2[a, b]$. If $p \in(a, b)$ is such that $f(p)=0$ and $f^{\prime}(p) \neq 0$. Then there exists a $\delta>0$ such that Newton's method generates a sequence $\left\{p_n\right\}_{n=1}^{\infty}$, defined by
$$
p_n=p_{n-1}-\frac{f\left(p_{n-1}\right)}{f^{\prime}\left(p_{n-1}\right)}
$$
converging to $p$ for any initial approximation
$$
p_0 \in[p-\delta, p+\delta]
$$

**Proof**
- The proof is based on analyzing Newton's method as the functional iteration scheme $p_n=g\left(p_{n-1}\right)$, for $n \geq 1$, with
$$
g(x)=x-\frac{f(x)}{f^{\prime}(x)}
$$
- 证明思路：证明上述问题满足 Fixed-Point Theorem 的所有假设即可证明任意初始近似 $p_{0}$ 都能收敛到 $p$。
- **首先**，证明在区间 $x \in(p-\delta, p+\delta)$ 上，$\left|g^{\prime}(x)\right| \leq k<1$。思路：基于性质 2，只要找到 $g^{\prime}(x)=0$ 即可。
	- 基于性质 1：Since $f^{\prime}$ is continuous and $f^{\prime}(p) \neq 0$, there exists a $\delta_1>0$, such that $f^{\prime}(x) \neq 0$ for $x \in\left[p-\delta_1, p+\delta_1\right] \subseteq[a, b]$
	- Thus $g$ is defined and continuous on $\left[p-\delta_1, p+\delta_1\right]$. Also 
$$
g^{\prime}(x)=1-\frac{f^{\prime}(x) f^{\prime}(x)-f(x) f^{\prime \prime}(x)}{\left[f^{\prime}(x)\right]^2}=\frac{f(x) f^{\prime \prime}(x)}{\left[f^{\prime}(x)\right]^2},
$$for $x \in\left[p-\delta_1, p+\delta_1\right]$, and, since $f \in C^2[a, b]$, we have $g \in C^1\left[p-\delta_1, p+\delta_1\right]$
- By assumption, $f(p)=0$, so
$$
g^{\prime}(p)=\frac{f(p) f^{\prime \prime}(p)}{\left[f^{\prime}(p)\right]^2}=0
$$
- 基于性质 2：Since $g^{\prime}$ is continuous and $0<k<1$, there exists a $\delta$, with $0<\delta<\delta_1$, and
$$
\left|g^{\prime}(x)\right| \leq k, \quad \text { for all } x \in[p-\delta, p+\delta] .
$$
- **其次**，证明在区间 $[p-\delta, p+\delta]$ 上，函数 $g$ 的值也映射在 $[p-\delta, p+\delta]$ 范围内。
	- If $x \in[p-\delta, p+\delta]$, the Mean Value Theorem some number $\xi$ between $x$ and $p,|g(x)-g(p)|=\left|g^{\prime}(\xi)\right||x-p|$. So
$$
|g(x)-p|=|g(x)-g(p)|=\left|g^{\prime}(\xi)\right||x-p| \leq k|x-p|<|x-p| .
$$
- Since $x \in[p-\delta, p+\delta]$, it follows that $|x-p|<\delta$ and that $|g(x)-p|<\delta$. Hence, $g$ maps $[p-\delta, p+\delta]$ into $[p-\delta, p+\delta]$.
- **总结**，关于 Fixed-Point Theorem 的所有假设已经满足了。所以定义如下的序列 $\left\{p_n\right\}_{n=1}^{\infty}$ 对于任意 $p_0 \in[p-\delta, p+\delta]$ 都将收敛到 $p$。
$$
p_n=g\left(p_{n-1}\right)=p_{n-1}-\frac{f\left(p_{n-1}\right)}{f^{\prime}\left(p_{n-1}\right)}, \quad \text { for } n \geq 1,
$$
>函数 $f$ 在区间 $[a,b]$ 上连续，$p$ 在开区间 $(a,b)$ 上
>性质 1：假定 $f (p)\neq 0$，存在 $\delta>0$ 使得在区间 $x \in[p-\delta, p+\delta]$ 上 $f(x) \neq 0$
>性质 2：假定 $f(p)=0$ 且 $k>0$ ，存在 $\delta>0$ 使得在区间 $x \in[p-\delta, p+\delta]$ 上 $|f(x)| \leq k$
>其中 $[p-\delta, p+\delta]$ 是 $[a,b]$ 的子集。


# 4_Secant & Regula Falsi Methods
Newton's method has a major weakness: the need to know the value of the derivative of $f$ at each approximation. 
Here, the convergence of the Secant method is much faster than functional iteration but slightly slower than Newton's method.

## Secant Method
because
$$
f^{\prime}\left(p_{n-1}\right)=\lim _{x \rightarrow p_{n-1}} \frac{f(x)-f\left(p_{n-1}\right)}{x-p_{n-1}}
$$ So using this approximation for $f^{\prime}\left(p_{n-1}\right)$ in Newton's formula gives
$$
p_n=p_{n-1}-\frac{f\left(p_{n-1}\right)\left(p_{n-1}-p_{n-2}\right)}{f\left(p_{n-1}\right)-f\left(p_{n-2}\right)}
$$

### Algorithm
To find a solution to $f(x)=0$ given initial approximations $p_0$ and $p_1$; tolerance $T O L$; maximum number of iterations $N_0$.
1. Set $i=2, q_0=f\left(p_0\right), q_1=f\left(p_1\right)$
2. While $i \leq N_0$ do Steps 3-6:
	3. Set $p=p_1-q_1\left(p_1-p_0\right) /\left(q_1-q_0\right)$. (Compute $\left.p_i\right)$
	4. If $\left|p-p_1\right|<T O L$ then OUTPUT $(p) ; \quad$ (The procedure was successful.) STOP
	5. Set $i=i+1$
	6. Set $p_0=p_1;q_0=q_1 ; p_1=p ; q_1=f (p)$
7. OUTPUT ('The method failed after $N_0$ iterations, $N_0=$ ', $N_0$ ); (The procedure was unsuccessful) STOP

## Summary the Secant & Newton's Methods
- The Secant method and Newton's method are often used to refine an answer obtained by another technique (such as the Bisection Method).
- Both methods require good first approximations but generally give rapid acceleration.

## The method of False Position
The method of False Position (also called Regula Falsi) generates approximations in the same manner as the Secant method, but it includes a test to ensure that the root is always bracketed between successive iterations.

In this illustration, the first three approximations are the same for both methods, but the fourth approximations differ: 
即因为对于 Secant Method，选择 p2 和 p3 的割线来计算 p4，但对于 False Position，由于 p3 和 p2 同号，p3 和 p1 的值异号，所以选择 p3 和 p1 的割线来计算 p4。
![image.png](https://image-host-1316314535.cos.ap-beijing.myqcloud.com/Obsidian%20Image/Secant%20and%20False%20Position.png)

### Algorithm
To find a solution to $f(x)=0$, given the continuous function $f$ on the interval $\left[p_0, p_1\right]$ (where $f\left(p_0\right)$ and $f\left(p_1\right)$ have opposite signs) tolerance $T O L$ and maximum number of iterations $N_0$.
1. Set $i=2 ; q_0=f\left(p_0\right) ; q_1=f\left(p_1\right)$.
2. While $i \leq N_0$ do Steps 3-7:
3. Set $p=p_1-q_1\left(p_1-p_0\right) /\left(q_1-q_0\right)$. (Compute $\left.p_i\right)$
4. If $\left|p-p_1\right|<T O L$ then OUTPUT $(p) ; \quad($ The procedure was successful $)$ : STOP
5. Set $i=i+1 ; q=f(p)$
6. If $q \cdot q_1<0$ then set $p_0=p_1 ; q_0=q_1$
7. Set $p_1=p ; q_1=q$
8. OUTPUT ('Method failed after $N_0$ iterations, $N_0=', N_0$ ); (The procedure was unsuccessful): STOP

## Summary
- The added insurance of the method of False Position commonly requires more calculation than the Secant method, ...
- just as the simplification that the Secant method provides over Newton's method usually comes at the expense of additional iterations.
