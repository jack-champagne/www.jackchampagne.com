---
title: Fibonacci, Spirals, and the Complex Plane
description: A blog post on fibonnaci
date: "2021-04-9"
draft: false
---

<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>

# The Fibonnaci Numbers

For the uninitiated, the Fibonnaci numbers are a set of numbers that are recusively defined by the following formula

\\[ F_{n+2} = F_{n+1} + F_n \\]

And we have the starting numbers \\( F_0 = 0, \quad F_1 = 1 \\). From this, many beautiful and surprising relationships can be created. Nature and other seemingly unrelated bits of math in general reveal themselves to have intimate relationships with this simple relation.

<p style="text-align:center"><iframe src="https://www.youtube.com/embed/ahXIMUkSXX0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></p>



This three video series by Vihart I remember watching very young. I was entranced by the beauty and the elegance between the math and nature.

Writing out the first few numbers, we have something like \\( 0, 1, 1, 2, 3, 5, 8, 23, \dots \\) and so on. If theres one thing that mathematicians love, it is generalizations and closed forms. In its current state, finding the n-th fibonnaci number \\( F_n \\) requires us to calculate \\( F_{n-1} \\) which requires us to calculate \\( F_{n-2} \\) which requires us to.... and so on. Finding the 100000th fibonnaci number sounds like something that will take forever. What if there was a better way? Using a little bit of linear algebra, we can show that there *is* a better way.

First let's set up the equations we have and what we know is true.

\\[ \begin{aligned}
F_{n+2} &= F_{n+1} + F_{n} \\\\
F_{n+1} &= F_{n+1}
\end{aligned} \\]

We have a system of linear equations which we can simplify into a matrix.
\\[
\\begin{bmatrix}
F_{n+2}\\\\
F_{n+1}
\\end{bmatrix}
= \\begin{bmatrix}
1 & 1\\\\
1 & 0
\\end{bmatrix} 
\\begin{bmatrix}
F_{n+1}\\\\
F_{n}
\\end{bmatrix}
\\]

\\[ \\overrightarrow{F}\_{n+1}  = 
\\mathbf{A} \\vec{F_{n}} \\]

Now if we want to find the n-th fibonnaci number, and we know \\( \\vec{F_1} = \\begin{bmatrix} 1\\\\0 \\end{bmatrix}\\), we can apply our tranformation matrix multiple times.

\\[ \begin{aligned}
\\vec{F_1} &= \\mathbf{A} \\vec{F_0}\\\\
\\vec{F_2} &= \\mathbf{A} \\vec{F_1} = \\mathbf{A} (\\mathbf{A} \\vec{F_0}) = \\mathbf{A}^2 \\vec{F_0} \\\\
&\\,\\,\\,\\,\\vdots \\\\
\\vec{F_k} &= \\mathbf{A}^k \\vec{F_0}
\end{aligned}
\\]

Now we are getting closer to a more direct way to computing fibonnaci directly. Unfortunately, taking higher and higher powers of a matrix is computationally the same problem as computing fibonnaci directly, a better approach to find higher powers of this matrix would be to first do something called **diagonalization** of this matrix. Diagonalization leverages special attributes of matricies called eigenvalues and eigenvectors so that we can preform the following factorization. If interested, read more [here on wikipedia](https://en.wikipedia.org/wiki/Diagonalizable_matrix)

\\[\\mathbf{A} = \\mathbf{S} \\mathbf{\Lambda} \\mathbf{S}^{-1} \\]

This factorization [(which I will not go through in detail)](https://en.wikipedia.org/wiki/Diagonalizable_matrix#Application_to_matrix_functions) allows us to take higher powers of a matrix easily, as so

\\[\\mathbf{A}^k = \\mathbf{S} \\mathbf{\Lambda}^k \\mathbf{S}^{-1}  \\]

The best part is, \\( \\Lambda \\) is a *diagonal matrix* which allows us to compute higher powers of it extremely easily, all we have to do is take higher powers of each of the elements to compute higher powers of the matrix.

So continuing, we will find the diagonalization of matrix A and we will find from that, a closed-form equation that will give us the n-th fibonnaci number. Here begins our diagonlizations procedure. You can skip this if you don't feel like checking my work but its just finding the eigenvalues and eigenvectors for our matrix.

First finding the eigenvectors:

\\[\\begin{aligned}
\\begin{vmatrix}    
1 - \\lambda & 1\\\\
1 & -\\lambda \\\\
\\end{vmatrix} &= 0\\\\\\\\
\\lambda^2 - \\lambda - 1 &= 0
\\end{aligned}
\\]

\\[\\lambda = \\frac{1 \\pm \\sqrt{(-1)^2 - 4(1)(-1)}}{2(1)} = \\frac{1 \\pm \\sqrt{5}}{2} \\]

Now finding the eigen vectors for \\(\\lambda_1 = \\frac{1 + \\sqrt{5}}{2} \\)
\\[
\\begin{aligned}
\\textbf{Nul}(\\mathbf{A} - \\tfrac{1 + \\sqrt{5}}{2} \\mathbf{I}) &= \\begin{bmatrix}
1 - \\frac{1 + \\sqrt{5}}{2} & 1 & 0\\\\
1 & - \\frac{1 + \\sqrt{5}}{2} & 0\\\\
\\end{bmatrix}\\\\
&= \\begin{bmatrix}
\\frac{1-\\sqrt{5}}{2} & 1 & 0\\\\
1 & - \\frac{1 + \\sqrt{5}}{2} & 0\\\\
\\end{bmatrix}\\\\
&= \\begin{bmatrix}
1 & -\\frac{1 + \\sqrt{5}}{2} & 0\\\\
1 & - \\frac{1 + \\sqrt{5}}{2} & 0\\\\
\\end{bmatrix}\\\\
&= \\begin{bmatrix}
1 & -\\frac{1 + \\sqrt{5}}{2} & 0\\\\
0 & 0 & 0\\\\
\\end{bmatrix}\\\\
\\end{aligned}
\\]

So, for \\(\\lambda_1: \\begin{bmatrix} \\frac{1 + \\sqrt{5}}{2}\\\\ 1 \\end{bmatrix}\\)

Now for \\(\\lambda_2 = \\frac{1 - \\sqrt{5}}{2} \\)
\\[
\\begin{aligned}
\\textbf{Nul}(\\mathbf{A} - \\tfrac{1 - \\sqrt{5}}{2} \\mathbf{I}) &= \\begin{bmatrix}
1 - \\frac{1 - \\sqrt{5}}{2} & 1 & 0\\\\
1 & - \\frac{1 - \\sqrt{5}}{2} & 0\\\\
\\end{bmatrix}\\\\
&= \\begin{bmatrix}
\\frac{1-\\sqrt{5}}{2} & 1 & 0\\\\
1 & - \\frac{1 - \\sqrt{5}}{2} & 0\\\\
\\end{bmatrix}\\\\
&= \\begin{bmatrix}
1 & -\\frac{1 - \\sqrt{5}}{2} & 0\\\\
1 & -\\frac{1 - \\sqrt{5}}{2} & 0\\\\
\\end{bmatrix}\\\\
&= \\begin{bmatrix}
1 & -\\frac{1 - \\sqrt{5}}{2} & 0\\\\
0 & 0 & 0\\\\
\\end{bmatrix}\\\\
\\end{aligned}
\\]

So, for \\(\\lambda_2: \\begin{bmatrix} \\frac{1 - \\sqrt{5}}{2}\\\\ 1 \\end{bmatrix}\\)

And now we can construct our diagonalized factorization per the diagonalization procedure. Eigenvectors and eigenvalues in corresponding columns

\\[
\\begin{aligned}
F_k = \\begin{bmatrix}
\\frac{1 + \\sqrt{5}}{2} & \\frac{1 - \\sqrt{5}}{2} \\\\
1 & 1\\\\
\\end{bmatrix}
\\begin{bmatrix}
\\frac{1 + \\sqrt{5}}{2} & 0 \\\\
0 & \\frac{1 - \\sqrt{5}}{2} \\\\
\\end{bmatrix}^k
\\begin{bmatrix}
\\frac{1 + \\sqrt{5}}{2} & \\frac{1 - \\sqrt{5}}{2} \\\\
1 & 1\\\\
\\end{bmatrix}^{-1}
\\end{aligned}
\\]

And finally we arrive at the equation after some multiplication called the Binet formula, it is a relationship using the [golden ratio](https://en.wikipedia.org/wiki/Golden_ratio) that corresponds to the \\(n\\)th fibonnaci number. Since we only care about the top equation as it is the fibonnaci relation, we will focus on that

\\[ \\begin{bmatrix}
F_{k}\\\\
F_{k-1}\\\\
\\end{bmatrix} = \\begin{bmatrix}
\\frac{\\left ( \\frac{1 + \\sqrt{5}}{2} \\right )^k - \\left ( \\frac{1 - \\sqrt{5}}{2} \\right )^k}{\\sqrt{5}} \\\\ \\dots
\\end{bmatrix} \\]

\\[ F_k = \\frac{\\left ( \\frac{1 + \\sqrt{5}}{2} \\right )^k - \\left ( \\frac{1 - \\sqrt{5}}{2} \\right )^k}{\\sqrt{5}} \\]

This is an amazing thing! Being able to find the \\(n\\)th fibonnaci number without expensive recursive addition with some exponentiation, addition, and division. The main topic of this blog post is not actually on this equation though, it is more about the following generalization. I'm sure you never thought about the 4.5th fibonnaci number, but now you see it might be calculable! We can plug any number into the exponent of the Binet equation. Naturally, I grab a calculator, type in the equation and I am disappointed when it spits back out error. It became quickly apparent why it did so. The second half of the equation \\( \\left ( \\frac{1 - \\sqrt{5}}{2} \\right )^k \\) becomes problematic without careful analysis.

Observe that the base of this exponentiation is negative! (\\(\\approx -0.6 \\)), the issue is when we try to raise a negative number to a fractional power, we can end up in a situation like this

\\[ (-0.6)^{4.5} = (-0.6)^{\frac{9}{2}} = \\sqrt{-(0.6)^9} \\]

And uh oh, we're in the complex plane. But this is still an answer, and here we have come to a stunning realization, fractional fibonnaci numbers are complex! This next part is some personal work exploring this and a few fun graphs. Before I go into my own analysis, I'd like to mention the inspiration for this post, Matt Parker and his youtube channel [(Stand Up Maths)](https://www.youtube.com/user/standupmaths) posted a video called [Complex Fibonacci Numbers?](https://www.youtube.com/watch?v=ghxQA3vvhsk) in June 2020 which inspired me to make this blog post. A lot of the ideas are borrowed from that video.

Let's graph it! But first, we need to figure out how we want to graph this function. A common approach it to use the x-axis for the real-valued part of the function and use the y-axis for the imaginary-valued part of the function.

\\[
\\begin{aligned} 
\\text{Re}\\left( \\frac{\\left ( \\frac{1 + \\sqrt{5}}{2} \\right )^k - \\left( \\frac{1 - \\sqrt{5}}{2} \\right )^k}{\\sqrt{5}} \\right) &= \\frac{\\left ( \\frac{1 + \\sqrt{5}}{2} \\right )^k}{\\sqrt{5}} - \\text{Re}\\left( \\frac{\\left( \\frac{1 - \\sqrt{5}}{2} \\right )^k}{\\sqrt{5}} \\right )\\\\
&= \\frac{\\left ( \\frac{1 + \\sqrt{5}}{2} \\right )^k}{\\sqrt{5}} - \\frac{\\left | \\frac{1 - \\sqrt{5}}{2} \\right |^k cos(k \\pi )}{\\sqrt{5}} \\\\
&= \\frac{\\left ( \\frac{1 + \\sqrt{5}}{2} \\right )^k - \\left | \\frac{1 - \\sqrt{5}}{2} \\right |^k cos(k \\pi )}{\\sqrt{5}}
\\end{aligned}
\\]

\\[
\\begin{aligned}
\\text{Img}\\left( \\frac{\\left ( \\frac{1 + \\sqrt{5}}{2} \\right )^k + \\left( \\frac{1 - \\sqrt{5}}{2} \\right )^k}{\\sqrt{5}} \\right) &=  \\text{Img}\\left( \\frac{\\left( \\frac{1 - \\sqrt{5}}{2} \\right )^k}{\\sqrt{5}} \\right )\\\\
&= \\frac{\\left | \\frac{1 - \\sqrt{5}}{2} \\right |^k sin(k \\pi )}{\\sqrt{5}}\\\\
\\end{aligned}
\\]

Sweet! Let's first look at the graph of the real component of this function. Let the x-axis in this graph be \\(k\\) or the index of the Fibonnaci number.
<p style="text-align:center"><iframe src="https://www.desmos.com/calculator/x526ua96f3?embed" style="border: 1px solid #ccc" frameborder=0></iframe></p>

And now for the complex component of this function. Again, let the x-axis be \\(k\\) or the index of the Fibonnaci number.
<p style="text-align:center"><iframe src="https://www.desmos.com/calculator/gflxigver1?embed" style="border: 1px solid #ccc" frameborder=0></iframe></p>

Finally, we graph both of them at the same time. We do this but defining a paramteric function where the parameter is \\(k\\) or the index of the fibonnaci number. Note that the axes now mean different things. The x-axis is the real component of the k-th fibonnaci number and the y-axis is the imaginary component of the k-th fibonnaci number. \\(k \\) varies from -20 to 20 and the green dot is \\(k-th \\) fibonnaci number and it varies from \\(k = -1.5\\) and \\(k = 4\\). And here's the final graph in all of its glory!
<p style="text-align:center"><iframe src="https://www.desmos.com/calculator/0xdpor9rhl?embed" style="border: 1px solid #ccc" frameborder=0></iframe></iframe></p>

Note how the zero-th fibonnaci number is at (0,0) and how this function passes through the point (1,0) twice! (three times if you include the negative first fibonnaci number!). The fibonnaci numbers spiral outwards when we look at negative fibonacci numbers, the \\(-1\\)st fibonacci number is \\(1\\) for example. The spiral looks like this zoomed out
<p style="text-align:center"><iframe src="https://www.desmos.com/calculator/npxv17zrye?embed" style="border: 1px solid #ccc" frameborder=0></iframe></p>

Feel free to play around with my graphs [here](https://www.desmos.com/calculator/k1ucihepaz), they are quite interesting to animate. You can also see some of the other details about fibonnaci and this particular thing in there.

Another analysis that I wanted do to immediately after I saw Matt Parker's video was looking at other recursively defined number sets. For one, there are the lucas numbers, where it is the same rule as the fibonnaci relation but the starting values are \\(2, 1\\) respectively. So the sequence goes \\(2, 1, 3, 4, 7, \\dots \\).
Here is a graph of that spiral!
<p style="text-align:center"><iframe src="https://www.desmos.com/calculator/mfmodotd8v?embed" style="border: 1px solid #ccc" frameborder=0></iframe></p>

The coolest part in my opinion is what happens when we let the initial parameters change and see the resulting graph, here is a graph that animates the changing initial conditions for the recurrence relation and the changing curve.
<p style="text-align:center"><iframe src="https://www.desmos.com/calculator/b1hjwvrtug?embed" style="border: 1px solid #ccc" frameborder=0></iframe></p>

Some of the next considerations that I could make and explore include what happens to this curve when we change the recurrance relation and add coefficients to the equation like this
\\[F_{n+2} = c_1 F_{n+1} + c_2 F_{n} \\]

How does this change the eigenvalues and eigenvectors? What if we change the initial conditions for this new relationship? These are all interesting explorations that can be done but for now, I leave this for something to appreciate and possibly explore yourself. Here is the [link](https://www.desmos.com/calculator/cycgxam5ec) to the second graph if you want to explore that one.