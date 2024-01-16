+++
title = 'Solving Calculus Problems With Linear Algebra'
date = 2024-01-15T15:35:16-06:00
draft = true
+++

## Introduction

Yesterday, while doom-scrolling youtube, I came across a
[video](https://youtu.be/jGP3t_17Xbg?si=fZuRJ1_RvxDnU0pa) 
from Michael Penn about finding an extremely high-order derivative
using Linear Algebra to avoid the bulk of what the work would otherwise
entail.

The problem being considered in the video was:
> Given $f(x) = e^{2x}\sin{3x}$
>
> Find $f^{(3030)}(0)$

## Solving the problem

The approach taken to solve this problem is given as follows.

Consider the following first few derivatives of f:

$f'(x) = \frac{d}{dx}[e^{2x}]\sin{3x} + \frac{d}{dx}[\sin{3x}]e^{2x}$
$ = 2e^{2x}\sin{3x} + 3e^{2x}\cos{3x}$

$f''(x)  = \frac{d}{dx}f'(x) = 2\frac{d}{dx}[e^{2x}]\sin{3x} + 2\frac{d}{dx}[\sin{3x}]e^{2x} + 3\frac{d}{dx}[e^{2x}]\cos{3x} + 3\frac{d}{dx}[\cos{3x}]e^{2x}$

$\ \ \ \ \ \ \ \ \ \ = 4e^{2x}\sin{3x} + 6e^{2x}\cos{3x} + 6e^{2x}\cos{3x} - 9e^{2x}\sin{3x}$

$\ \ \ \ \ \ \ \ \ \ = -5e^{2x}\sin{3x} + 12e^{2x}\cos{3x}$

If you don't see the pattern, do this a few more times. What you'll notice is that
all functions that can be produced by differentiation are of the form:

$ae^{2x}\sin{3x} + be^{2x}\cos{3x}$ for some $a,b \in \mathbf{R}$

Moreover, the set of all as and bs satisfying the above lie in the span of
$e^{2x}\sin{3x}$ and $e^{2x}\cos{3x}$.

So we define a vector space $V = \text{span} \\{ e^{2x}\sin{3x}, e^{2x}\cos{3x}\\}$

And we create a map called T where $e^{2x}\sin{3x} \stackrel{T}{\mapsto}$
$ \begin{bmatrix} 1 \cr 0 \end{bmatrix}$ and
$ e^{2x}\cos{3x} \stackrel{T}{\mapsto} \begin{bmatrix} 0 \cr 1 \end{bmatrix}$.

So $(T \circ f)(x) = (1, 0) \in V$

$(T \circ f')(x) = (2, 3) \in V$

$(T \circ f'')(x) = (-4, 12) \in V$

Now we want a transformation D which mimics the derivative in $V$.

As can be seen above from choice of basis and $f'$,
<div>
$D\begin{bmatrix} 1 \\ 0 \end{bmatrix} = \begin{bmatrix} 2 \\ 3 \end{bmatrix}$
</div>

$D(0, 1)$ corresponds to $\frac{d}{dx}e^{2x}\cos{3x} = 2e^{2x}\cos{3x} - 3e^{2x}\sin{3x}$
$ = -3e^{2x}\sin{3x} + 2e^{2x}\cos{3x}$

Thus, 
<div>
$D\begin{bmatrix} 0 \\ 1 \end{bmatrix} = \begin{bmatrix} -3 \\ 2 \end{bmatrix}$
</div>

Therefore, 
<div>
$D = \begin{bmatrix} 2 & -3 \\ 3 & 2 \end{bmatrix}$
</div>

is the transformation in our constructed vector space which encodes the derivative
in $\mathbf{R}^2$.

Just to verify, let's use $D$ to calculate the derivative of $f'$.

<div>
$T(f') = \begin{bmatrix} 2 \\ 3 \end{bmatrix} \in V$
</div>

<div>
$(DT)f' = D(T(f')) = D \begin{bmatrix} 2 \\ 3 \end{bmatrix} = \begin{bmatrix} 2 & -3 \\ 3 & 2 \end{bmatrix} \begin{bmatrix} 2 \\ 3 \end{bmatrix} = \begin{bmatrix} 2(2) - 3(3) \\ 3(2) + 2(3) \end{bmatrix} = \begin{bmatrix} -5 \\ 12 \end{bmatrix}$
</div>

And

<div>
$T^{-1} \begin{bmatrix} -5 \\ 12 \end{bmatrix} = T^{-1}(-5i + 12 j) = -5T^{-1}(i) + 12T^{-1}(j) = -5e^{2x}\sin{3x} + 12e^{3x}\cos{3x} = f''(x)$
</div>

It seems to be working! Cool!

Notice that in that last line we use linearity of $T^{-1}$ without ever showing
that $T$ or $T^{-1}$ is linear. I won't go into detail, but $T$ is linear and a
bijection which is which $T^{-1}$ is also linear. [Here's](https://math.stackexchange.com/questions/1645103/show-that-an-inverse-of-a-bijective-linear-map-is-a-linear-map)
a proof if you're skeptical.

Notice that $\frac{d}{dx}^{(3030)}$ corresponds to $D^{3030}$, however, this matrix
power computation is just as hard to compute. So we'd like to map into a space where
the transformation represented by our $D$ can be written in another basis in such
a way that the transformation w.r.t. that new basis is a diagonal matrix since:

<div>
$\begin{bmatrix} a & 0 \\ 0 & b \end{bmatrix}^n = \begin{bmatrix} a^n & 0 \\ 0 & b^n \end{bmatrix}$ for any choice of $n \in \mathbf{N}$
</div>

How can we do this? By finding the eigenvectors of our matrix $D$ and doing a change
of basis to those eigenvectors!

Characteristic Equation: $\det(D - \lambda I) = 0$

<div>
$A = D - \lambda I = \begin{bmatrix} 2 - \lambda & -3 \\ 3 & 2 - \lambda \end{bmatrix}$
</div>

$\det(A) = (2 - \lambda)^2 + 9 = 0$

$\implies (2 - \lambda)^2 = -9$

$\implies 2 - \lambda = \pm 3i$

$\implies \lambda = 2 \mp 3i$

$\lambda_1 = 2 - 3i$ and $\lambda_2 = 2 + 3i$ are our eigenvalues

Using these eigenvalues, we can find eigenvectors $v_1, v_2$ as follows:

$(D - \lambda_1 I)v_1 = 0$ and $(D - \lambda_2 I)v_2 = 0$

<div>
$(D - \lambda_1 I)v_1 = \begin{bmatrix} 2 - \lambda_1 & -3 \\ 3 & 2 - \lambda_1 \end{bmatrix}v_1 = \begin{bmatrix} 3i & -3 \\ 3 & 3i \end{bmatrix}v_1 = B_1v_1 = 0$
</div>

<div>
$(D - \lambda_1 I)v_1 = \begin{bmatrix} 2 - \lambda_2 & -3 \\ 3 & 2 - \lambda_2 \end{bmatrix}v_1 = \begin{bmatrix} -3i & -3 \\ 3 & -3i \end{bmatrix}v_1 = B_2v_2 = 0$
</div>

$B_1v_1 = 0$ can be solved by gaussian elimatination and back substitution.

<div>
$B_1 \stackrel{R_2 := R_2- \frac{1/i}R_1}{\longrightarrow} \begin{bmatrix} 3i & -3 \\ 0 & -3i-\frac{3}{i} \end{bmatrix} = \begin{bmatrix} 3i & -3 \\ 0 & \frac{-3i^2-3}{i} \end{bmatrix} = \begin{bmatrix} 3i & -3 \\ 0 & -3\frac{i^2+1}{i} \end{bmatrix} = \begin{bmatrix} 3i & -3 \\ 0 & 0 \end{bmatrix} = U_1$ 
</div>

and $U_1v_1 \implies 3iv_1 - 3v_2 = 0 \implies v_1 = -iv_2$

This means there are infinite eigenvectors with eigenvalue $\lambda_1$ and
one such vector is $(1, -i)$.

$B_2v_2 = 0$ can be solved the same way to get an eigenvector $(1, i)$.
We can know this without doing the computation since complex eigenvectors come in
conjugate pairs.

We now define a transformation $L$ from $V$ to $V'$ where $(1, 0) \in V \stackrel{L}{\mapsto} (1, i) \in V'$ 
and $(0, 1) \in V \stackrel{L}{\mapsto} (1, -i) \in V'$.

This map $L$ is clearly:
<div>
$L = \begin{bmatrix} 1 & 1 \\ i & -i \end{bmatrix}$
</div>

And it's inverse can be found with the 2-by-2 inverse formula 
<div>
$\det \begin{bmatrix} a & b \\ c & d \end{bmatrix} = \frac{1}{\det \begin{bmatrix} a & b \\ c & d \end{bmatrix}} \begin{bmatrix} d & -b \\ -c & a \end{bmatrix}$
</div>

Thus, 
<div>
$L^{-1} = \frac{1}{-i - i} \begin{bmatrix} -i & -1 \\ -i & 1 \end{bmatrix} = \frac{i}{2} \begin{bmatrix} -i & -1 \\ -i & 1 \end{bmatrix} = \begin{bmatrix} \frac{1}{2} & \frac{-i}{2} \\ \frac{1}{2} & \frac{i}{2} \end{bmatrix}$
</div>

is our inverse transformation from $V'$ to $V$.

And by change of basis,

<div>
$LDL^{-1} = \begin{bmatrix} 1 & 1 \\ i & -i \end{bmatrix} \begin{bmatrix} 2 & -3 \\ 3 & 2 \end{bmatrix} \begin{bmatrix} \frac{1}{2} & \frac{-i}{2} \\ \frac{1}{2} & \frac{i}{2} \end{bmatrix} = \begin{bmatrix} 1 & 1 \\ i & -i \end{bmatrix} \begin{bmatrix} 1 - \frac{3}{2} & -i - \frac{3i}{2} \\ \frac{3}{2} + 1 & \frac{-3i}{2} + i \end{bmatrix} = \begin{bmatrix} 1 & 1 \\ i & -i \end{bmatrix} \begin{bmatrix} \frac{-1}{2} & \frac{-5i}{2} \\ \frac{5}{2} & \frac{-i}{2} \end{bmatrix}$
</div>

<div>
$\implies LDL^{-1} = \begin{bmatrix} 2 & -3i \\ -3i & 2 \end{bmatrix} = P$
</div>

$P$ is our transformation in $V'$ that encodes our derivative.
(Note that the P here is different from the P used in the video that inspired
this post)

<div>
$P \stackrel{R_2 := R_2 + \frac{2i}{3}R_1}{\longrightarrow} \begin{bmatrix} 2 & -3i \\ 0 & 2-\frac{2i}{3}3i \end{bmatrix} = \begin{bmatrix} 2 & -3i \\ 0 & 4 \end{bmatrix} \stackrel{R_1 := R_1 + \frac{3i}{4}R_2}{\longrightarrow} \begin{bmatrix} 2 & 0 \\ 0 & 4 \end{bmatrix}$
</div>

Recall that $T(f') = (2, 3) \in V$.

And notice, $L(2,3) = (2 + 3, 2i - 3i) = (5, -i) \in V'$

Moreover, $P(5, -i) = (10, -4i) \in V$

Then, 

<div>
$L^{-1}(10, -4i) = \begin{bmatrix} \frac{1}{2} & \frac{-i}{2} \\ \frac{1}{2} & \frac{i}{2} \end{bmatrix} \begin{bmatrix} 10 \\ -4i \end{bmatrix} = \begin{bmatrix} 3 \\ 7 \end{bmatrix} \in V$
</div>

And finally, $T^{-1}(3, 7) = $ WRONG! fix this grayson.


<!-- $\implies 3iv_{1,1} - 3v_{1,2} = 0 \wedge 3v_{1,1} + 3iv_{1, 2} = 0$ -->
