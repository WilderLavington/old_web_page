# Tutorials
This page contains links to a couple of the code along tutorials I have created over the last couple years, either for courses, teaching assistantships, or just for fun! If something looks interesting, just click on the title to be re-directed to the page containing the notebook. Everything is ordered by subject, and the year that I created it, bon appetite.

## Reinforcement Learning Implimentations on a 2D gridworld

[Maximum entropy policy gradients:](https://github.com/WilderLavington/WilderLavington.github.io/blob/master/tutorial/Maximum_Entropy_Policy_Gradients/Maximum_Entropy_Policy_Gradients.md)     
This tutorial reviews a simple implimentation of maximum entropy policy gradients applied to a simple, fully observed markov descion proccess. This is the first in a series of tutorials that leads up to eventually implimenting a solution of to a high dimentional reinforcement in a probabalistic programming language, and reviews all the advantages and pitfall of this methodology.

[Generalized Advantage Estimation:](https://github.com/WilderLavington/WilderLavington.github.io/blob/master/tutorial/Generalized%20Advantage%20Estimation/Generalized%20Advantage%20Estimation.md)  
This tutorial sets up the generalized advantage estimator described in https://arxiv.org/abs/1506.02438, within a simple, discrete, fully observed MDP setting. The value function is trained using an iterative approach such as Adam or SGD on a linear function applied to the MSE between the current value function approximation, and the empirical value function recovered from the previous trajectory. 

[Trust Region Update for Value Function Approximation:](https://github.com/WilderLavington/WilderLavington.github.io/blob/master/tutorial/Trust%20Region%20Updates%20for%20Generalized%20Advantage%20Estimation/Trust%20Region%20Updates%20for%20Generalized%20Advantage%20Estimation.md)   
In order to improve the stability of the value function approximation, we now incorporate a trust region update into the primal problem. Instead of exactly solving the problem via langrangian duality, we set a specific value for the lagrange multiplying and apply minimization. In the next section we will solve this problem more principly.

## Introduction to Machine Learning Tutorials:

[Fundementals of Probability and Linear Algebra in Numpy:](https://github.com/WilderLavington/WilderLavington.github.io/tree/master/tutorial/Intro%20To%20Machine%20Learning%20Tutorial%201)  
This tutorial reviews some of the basic requirements to understand most machine learning algorithms that are used in practice today. Although the lecture slides are not meant to stand alone, they do list most of the important definitions and rules that are a must know for any would be machine learners and artificial intelligence aficionados. The tutorial also includes a breif review of numpy by going over a few cannonical practice problems in probability, linear algebra, and numerics. 
