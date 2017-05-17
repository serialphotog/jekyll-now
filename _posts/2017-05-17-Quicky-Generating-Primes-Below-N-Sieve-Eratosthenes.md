---
layout: post
title: Quickly Generating Primes Below n With the Sieve of Eratosthenes
category: Development
tags: [math, computer science, python, programming]
---

The uses for prime numbers in computer science are nearly endless. They are useful for everything from hashing, cryptology, factorization and all sorts of applications in-between.

There exists a great number of algorithms that allow us to quickly generate primes, but today we are going to take a look at a popular method known as a **prime sieve**. There are a number of different implementations of prime sieves, but one of the simplest to implement is known as the *[Sieve of Eratosthenes](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes)*. This algorithm is great for quickly generating smaller prime numbers (but it may not be the best choice for generating very large primes).

# How it Works

In general, the Sieve of Eratosthenes works by generating a list of number from **2 to n**. The algorithm will then work through the list, marking all the composite numbers. Here is a more detailed breakdown of the implementation:

1. Create a list of integers from **2 to n**. We start at 2 because it's the smallest prime
2. Set **p=2**
3. Iterate over the multiples of **p** by counting to **n** from **2p** in increments of **p**. These are the numbers that get marked as composites in the list.
4. Find the first number greater than **p** in the list that is not marked. If one does not exist, we are done. If one does exist, however, set **p** to this new value and repeat from step 3. 

This method has a complexity of **O(NloglogN)**.

# Implementation in Python

Let's take a look at how we can implement a Sieve of Eratosthenes in Python:

{% highlight python %}
def get_primes():
  D = {}
  p = 2
  
  while 1:
    if p not in D:
      yield p
      D[p*p] = [q]
    else:
      for q in D[p]:
        D.setdefault[q+p, []).append(q)
      del D[p]
    p += 1
{% endhightlight %}

For this implementation I have modified things a bit to yield an infinite prime generator.

Let's take a moment to consider an example of how this could be used in a practical scenario. Let's take, for example, [problem 10 from Project Euler](https://projecteuler.net/problem=10), which asks that we find the sum of all the primes below 2 million. Using our *gen_primes()* method we can easily solve this with the following:

{% highlight python %}
primes = gen_primes()
print(sum(itertools.takewhile(lambda x: x < 2000000, primes)))
{% endhighlight %}

## Another Practical Example

Before I wrap up this post, let's consider just one more practical use for our *gen_primes()* method. Assume that we needed to find out what the n<sup>th</sup> prime is. For the purpose of example, let's just say we want to find the 500<sup>th</sup> prime number. It turns out this can be done easily with the following:

{% highlight python %}
primes = gen_primes()
print(next(itertools.islice(primes, 500, None), None))
{% endhighlight %}

Running this will reveal that the 500<sup>th</sup> prime number is 3,581. 

# Wrap Up

As I hope you can see, the Sieve of Eratosthenes is a simple way to generate prime numbers that can prove useful in a number of situations. I hope you've found this helpful! 
