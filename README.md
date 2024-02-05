## Intro
Step-by-step solution to the Pennylane Code Camp challenge question covered in [Quantum Programming Job Interview Q&A | PennyLane Tutorial](https://www.youtube.com/watch?v=6fM8FatYWt8) video.

## The question:
Solve for the gate operation `U` such that the whole algorithm depicted in the picture below is equivalent to a SWAP gate.

![Pennylane Problem](pennylane_problem.png)

In the video, Guillermo presents two methods to solve this. The first, covered briefly, is via machine learning: rewrite `U` as a series of several parameter-dependent parts, then minimize the loss function (SWAP gate minus our current algorithm guess) with respect to the parameter values. The second method, explained in more detail, is via compilation, using clever substitutions and the fact that the SWAP gate can be decomposed into simpler parts. 

Here I present a slightly different explanation of the latter method. It is a less elegant solution, but doesn't require knowing the decomposition and requires less cleverness in substitutions. See the notebook `solve_pennylane.ipynb` for a quick demonstration using numpy.

## The Solution:
We can write out the representation of the problem like this:

$$A B U C U B = S$$

where $A$ is the CNOT gate (qubit 1 control), $B$ is the parallel Hadamard gates, $U$ is our gate to solve for, $C$ is a CNOT (qubit 2 control), $S$ is the SWAP gate.

Let's try to isolate $U$. \
We are particularly using the fact that these are all unitary matrices, with $A A^{-1} = A^{-1}A= 1$. \

First, multiply both sides from the left by $A^{-1}$, then by $B^{-1}$:

$$ U C U B = B^{-1} A^{-1} S $$

Next, multiply both sides from the right by $B^{-1}$:

$$ U C U = B^{-1} A^{-1} S B^{-1} $$

Now let's do one substitution. Let $U'=CU$:

$$ C^{-1} U' U' = B^{-1}ADB^{-1} $$

Multiplying both sides with $C$ and taking the square root we get

$$ U' = \sqrt{CB^{-1}ADB^{-1}} $$

and subsituting back, we get the final analytical form of $U$:

$$ U = C^{-1} \sqrt{CB^{-1}ADB^{-1}} $$


Refer to the notebook for a quick demonstration solving this in python using just numpy and scipy. Refer to the pdf for a very explicit example of how you can determine the matrix form of these 2-qubit gates.