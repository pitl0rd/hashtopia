## Summary

A Markov chain is a mathematical system that undergoes transitions from one state to another according to certain probabilistic rules. The defining characteristic of a Markov chain is that no matter how the system arrived at its current state, the possible future states are fixed. In other words, the probability of transitioning to any particular state is dependent solely on the current state and time elapsed.

Markov chains have many applications as mathematical models of real-world systems that exhibit random behavior. They are used in a variety of fields, including economics, computer science, and biology.

In a Markov chain, a set of possible states is defined, and a transition matrix specifies the probabilities of transitioning from one state to another. The transition matrix is used to determine the probability of moving from the current state to any possible future state. The process of moving from one state to another is called a "step," and a sequence of steps is called a "path."

Markov chains can be represented visually using directed graphs, with the states represented as nodes and the transitions as directed edges connecting the nodes. The weight of each edge represents the probability of transitioning from one state to another.

Markov chains have several important properties, including the assumption of "memorylessness," meaning that the probability of transitioning to any particular state is dependent only on the current state and not on the sequence of states that preceded it. This property is known as the "Markov property." Another important property is "detailed balance," which states that the probability of transitioning from state A to state B is equal to the probability of transitioning from state B to state A.

Markov chains are often used to model systems that exhibit random behavior, such as the movement of a particle through a series of connected states, the flow of traffic on a network, or the evolution of a population over time. They are also used to solve optimization problems and make predictions about the behavior of systems.

## Password Cracking Applications

Markov Chains are created, for our password cracking purposes, by statistical analysis of a large list of passwords/words (i.e. the [RockYou](https://hashtopia.net/7.+Wordlists/RockYou) password dataset). The resultant analysis of these words and their per-position character frequency/probability are stored in a table. This table is referenced when performing brute-force/mask attacks to prevent having to generate password candidates in a sequential order, which is very inefficient. Instead, the most common characters are attempted first in order of preceding character probability. So let's see sequential brute-force ?a?a?a?a with out Markov Chains applied:

```
aaaa  
aaab  
aaac  
aaad  
aaae  
aaaf  
aaag  
aaah  
```

Now the same brute-force attack with Markov Chains applied:

```
sari  
mari  
lari  
aari  
cari  
bari  
103  
pari  
2ari
```

Markov Chains predict the probability of the next character in a password based on the previous characters, or context characters. It's that simple.

## Resources

Fast Dictionary Attacks on Passwords Using Time-Space Tradeoff  
[http://www.cs.utexas.edu/Nshmat/shmat_ccs05pwd.pdf](http://www.cs.utexas.edu/Nshmat/shmat_ccs05pwd.pdf)  
OMEN: Faster Password Guessing Using an Ordered Markov Enumerator  
[https://hal.inria.fr/hal-01112124/document](https://hal.inria.fr/hal-01112124/document)

[[1. Concepts|Concepts]]
[Home](https://hashtopia.net/Home)

 #advanced #research  #education 