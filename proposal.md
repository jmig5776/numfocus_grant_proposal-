# Proposal Title:- Improvement of Polynomial module of SymEngine

## Two Sentence Summary of Proposal:
Currently, SymEngine (part of SymPy) has a polynomial module which supports Univariate polynomial, and it has some basic functionalities, but plenty essential methods are not implemented. I propose to expand the features of Univariate Polynomial such as apart, cancel, etc and many more and make it better while considering the performance of it also.


## Description of Proposal: No more than 750 words (4,500 characters max)

As the existing Polynomial module of SymEngine is easily extensible and currently lacking some important manipulation functionalities. The idea is to implement most of the polynomial manipulation features such as apart, cancel, square-free decomposition, and Groebner basis, etc.  The idea is to make the polynomial module of SymEngine more functional and efficient.
**We will use external libraries where possible.**.
	For my project, I plan to implement the following algorithms for polynomial module:
* Implement cancel() - Cancels common factors from the numerator and denominator of a rational function. It depends on the GCD algorithm. Although there is existing pull request is started to complete it [#1620](https://github.com/symengine/symengine/pull/1620).
* Implement apart() - Decomposes a univariate rational function f with integer coefficients into partial fractions. The algorithm of Bronstein's full, partial fraction decomposition algorithm will be implemented. This has also been started to implement on [#1617](https://github.com/symengine/symengine/pull/1617).
* Implement square-free decomposition - The fast algorithm of Yun for computing square–free decompositions in domains of characteristic zero for multivariate polynomials. The cost of computing square-free decomposition is equivalent to the computation of the greatest common divisor of f and its derivative. This is dependent on the GCD algorithm.
* Implement Groebner Basis - The method of Groebner bases is a powerful technique for solving problems in commutative algebra. Pure description of implementation is [here](https://mattpap.github.io/masters-thesis/html/src/groebner.html). Groebner bases provide a uniform approach for solving problems that can be expressed in terms of systems of multivariate polynomial equations. It has many practical applications. The algorithm used for computation is Buchberger's algorithm.
* Implement Polynomial roots with the system of polynomials with the help of implemented Groebners basis and many more. 
* If time persists, then improvement of the multivariate polynomial will start after discussing with Ondrej and Isuru.

## Benefit to Project/Community: No more than 400 words (2,500 characters max)

SymPy is a Free, Python-based, Lightweight library, and many projects are using SymPy for their analytical and mathematical work. SymEngine being its optional core written in C++, aiming to become the fastest computer algebraic system ever.
SymEngine currently lacks essential functionalities in the polynomial module, which is essential in achieving SymEngine's goal of being the fastest CAS ever. Now it has polynomial module focusing univariate systems, and implementing fast polynomial manipulation functions are very necessary for expanding its usage. We can use Piranha's polynomial module, but it has complicated code and does not compile with old compilers. I aim to implement the algorithms in SymEngine itself, and many functions do not exist in Piranha.

There are many areas in the field of mathematics where SymPy is doing a much better job, and one of them is handling polynomials, but making it fastest, SymPy has to take help from the SymEngine module. Manipulation of polynomials is one of the many great ideas to have evolved in mathematics. They are among my favorite mathematical topics. SymPy provides me an excellent platform to hone my programming skills and put my mathematical skills to good use for improving SymEngine. I have been working with Sympy, and I had done Google Summer Of Code 2019 in Sympy and modified the documentation and code of Sympy mainly in the solvers module. I had also been connected with the code of SymEngine and started PRs to improve the existing codebase.

 Handling and manipulating polynomials are used widely in the sciences, such as engineering, physics, and economics, as well as in studies of mathematics. Mathematical descriptions of extracting common factors or distributing factors or solving polynomials using a Groebner basis are very good algorithms.

Currently, SymEngine has a polynomial module which is lacking many many functionalities, and the methods that will be implemented in this project will use external dependencies to save time and maintain performance. In the near future, if it will have its methods, then these methods will be deprecated.


## Amount Requested: 3000$
## Brief Budget Justification:-

I understand that when you are giving money, you want to know exactly where your money is going, and I pledge to be clear and transparent when it comes to spending your contributions. Lots of the money offered will go into my academic fee.

3000$ will cover for 12 weeks, and I plan to devote 45-50 hours per week for the project.


## Timeline of Deliverables: 
### Week 1 - 3
 * Implement cancel and apart algorithm.
 
This is how both algorithms will be implemented.
1. Implement cancel from SymPy and write test cases also
2. Implement div.
3. Implement apart_undetermined_coeffs.
Both the cancel and apart will be implemented with the reference from SymPy, and then these will be designed and assembled in Basic.
Cancel - Cancels common factors from the numerator and denominator of a rational function. It depends on the GCD algorithm.
Apart - Decomposes a univariate rational function f with integer coefficients into partial fractions. The algorithm of Bronstein's full, partial fraction decomposition algorithm will be implemented, and test cases will be written.

Although the implementation of cancel and apart is being started in these Pull requests.
[#1620](https://github.com/symengine/symengine/pull/1620), [#1617](https://github.com/symengine/symengine/pull/1617).
    	 
### Week 4

* Implement square-free decomposition

The fast algorithm of Yun for computing square–free decompositions in domains of characteristic zero for multivariate polynomials. The cost of square-free computing decomposition is equivalent to the computation of the greatest common divisor of f and its derivative. This is dependent on the GCD algorithm.
Link for Yun Algorithm 
Working principle
```
a_0 = gcd(f, f');   b_1 = f/a_0;   c_1 = f'/a_0;   d_1 = c_1 - b_1';   i = 1;
iterate until b = 1
a_i = gcd(b_i, d_i);   b_(i+1) = b_i/a_i;   c_(i+1) = d_i/a_i;   i = i + 1;    d_i = c_i - b_(i+1)';
return a_1, a_2, ... ,a_(i-1)
```
### Week  5 -7

• Implement Groebner basis

After going through this [link]()https://mattpap.github.io/masters-thesis/html/src/groebner.html Grobner basis is easy to implement.
Check for Groebner Basis.
Given a set of polynomials G, one can check if G is a Groebner basis in a finite number of steps using the generalized division algorithm implemented.
Given two polynomials f and g, to test whether they form Grobner basis.
1. Find s_poly(defined below) of f and g.
2. Find the remainder of s_poly with respect to [f, g] using reduced().
3. If zero, then f and g form a Groebner basis.
4. If not, then [f, g, s_poly(f, g)] form Groebner basis, which can be shown by taking s_poly pairwise and computing its remainder with the extended array and all turn out to be zero.
This can be extended for larger arrays.
* Algorithm
Methods to be implemented for the algorithm:
1. LM(): Returns the leading monomial.
2. LT(): Returns the leading term.
3. Also needed will be lcm() and expand().
We then define the notion of s_poly.
```
Polynomial s_poly(const Polynomial a, const Polynomial b){
	return expand(lcm(LM(f), LM(g))*(1/LT(f)*f - 1/LT(g)*g));
}
```
Note the importance of introducing ordering of monomials in computation of s_poly.
Once the above methods are implemented, then an improved version of Buchberger's algorithm will be done from here.
The same is also implemented in Groebner() of SymPy here.

### Week 8 - 10

* Implement a solving system using Groebner basis

It should be noted that for a linear system of polynomials, this reduces to Gauss-algorithm and hence can be applied to solve that system. In SymPy, it is noted that solving using this method is faster than the Gauss-Jordan implemented using solve()
```
>>> F = [x + 5*y - 2, -3*x + 6*y - 15]
>>> %timeit Groebner(F, x, y)
100 loops, best of 3: 5.15 ms per loop
>>> %timeit solve(F, x, y)
10 loops, best of 3: 22.7 ms per loop
```

# Week 11 - 12 End
Complete leftovers and Add documentation and improvement for multivariate starts.
