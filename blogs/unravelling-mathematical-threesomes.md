---
layout: post_no_side_bar
title: Unravelling Mathematical Threesomes
date: June 28, 2024
tags: ["pythagoras", "quadratic formula", "pythagorean triples"]
---

> Title is inspired by a recent Netflix watch: [The Secret Diary of a Call Girl](https://www.netflix.com/ie/title/70136150?source=35)

# Introduction
Dearest Gentle Reader, when I was in primary school some 10+ years that I can't accurately calculate again (the irony of it all), I had some assignment that involved calculating a single value out a pythagorean triple, two of the values were provided for ease.

![img.png](../images/pythagorean_two_values.png)


However, one particular case did not appear properly as the page had folded during print and messed up the value leaving me with only one value needing to find the other two.

![img.png](../images/pythagorean_single_value.png)

This article is based on me reminiscing ecstatically on a Friday night the experience I had figuring it out over a decade ago:
**how does one find the remaining two elements if given one element in a pythagorean triple?**


# What is a pythagorean triple?
Pythagoras theorem is applied when computing one end of the three sides of a right-angled (90 degree) triangle. 

![img.png](../images/triangle-right-angled-normenclature.png)

A [pythagorean triple](https://mathworld.wolfram.com/PythagoreanTriple.html) is basically three elements (a, b, c) that satisfy:

{% raw %}
$$a^2 + b^2 = c^2$$ 
{% endraw %}


# Starting simple with natural numbers
Natural numbers are all positive numbers excluding 0. This defines a metric system we can understand representing what a valid triangle would be constrained by.
Therefore, all numbers for the start of this exercise will be bounded within the [natural number space](https://en.wikipedia.org/wiki/Natural_number).

{% raw %}
\\[ \forall b \in {\displaystyle \mathbb {N} } \\]
{% endraw %}

{%raw%}$$a$${%endraw%} will represent either the adjacent or opposite, it does not matter which and {%raw%}$$c$${%endraw%} will be the hypotenuse/diagonal.
To ease out the discussion in later parts of this article, these will exist in the positive [real number space](https://en.wikipedia.org/wiki/Real_number).

{% raw %}
\\[ \forall a,c \in {\displaystyle \mathbb {R} } \ni \\{a \geq 1 \;and\; c > a\\} \\]
{% endraw %}


# Solving the problem
As earlier noted, a pythagorean expression is represented by
{% raw %}
$$a^2 + b^2 = c^2$$
{% endraw %}

If we take any edge of the triangle asides the hypotenuse, the subject of the formula we arrive at is:
{% raw %}
$$b^2 = c^2 - a^2$$
{% endraw %}

Factorising the expression leads to:
{% raw %}
$$b^2 = (c + a) * (c - a) $$
{% endraw %}

Whenever, we have expressions in this form, we can take a factorization similar to computing the factors of a number. 
In this case, I factorise the expression to satisfy the equality as such:
{% raw %}
$$c + a = b^2$$

$$c - a = 1$$
{% endraw %}

In doing so, I also define some expression that assumes there is a minimum distance {%raw%}$$k$${%endraw%} which is a whole positive number that is based on the difference of c and a.

{% raw %}\\[ \forall k \in {\displaystyle \mathbb {N} } \ni k: k = c - a \\] {% endraw %}

In this case, we start off with 1 between the hypotenuse and the opposite. This will be pivotal in later exercises on defining what I call a "pythagorean distance" which I refer to as {%raw%}$$k$${%endraw%}.

For this example with a "pythagorean distance" of 1, we will sum the expression to eliminate {% raw %} $$a$$ {% endraw %} and compute the expression for the hypotenuse.

{% raw %}
$$c + a = b^2$$

$$c - a = 1$$

--------------

$$2* c = b^2 + 1$$
{% endraw %}

This expands to the following expression for calculating the hypotenuse of a pythagorean triple.

{% raw %}
$$c = \frac{1}{2} (b^2 + 1)$$
{% endraw %}


Doing the same with {%raw%}$$a$${%endraw%} gives
{% raw %}
$$a = \frac{1}{2} (b^2 - 1)$$
{% endraw %}


However, in using this formula, we must consider the earlier equalities:

{% raw %}
$$c + a = b^2$$

$$c - a = 1$$

{% endraw %}

When considering the equalities, we must enforce that {%raw%}$$c + a > c - a$${%endraw%}. This is because in our natural number space without 0, we cannot have an addition greater or equal to a subtraction.

Substituting that {%raw%}$$c + a = \frac{b^2}{k}$${%endraw%} and  {%raw%}$$k = c - a = 1$${%endraw%}, we have that {%raw%}$$b ^ 2 > 1$${%endraw%}, implying {%raw%}$$b > 1$${%endraw%}

{% raw %}
\\[ \forall a,c \in {\displaystyle \mathbb {N} }, c+a > c-a  \\]

\\[ \therefore \frac{b^2}{k} > k \\]

\\[ \therefore b^2 > k^2 \\]

\\[ \therefore b > k \\]
{% endraw %}

# Testing the assertions
Now, let's test our formula with some simple math and python.

> NOTE: The code down here is better than spaghetti but pasta is pasta :-P

```python
import math
from fractions import Fraction

distances = 100
n = 100

def csv_print(*args, log):
    log.append(','.join(map(str, args)))
    # print(*args, sep=",")


for k in range(1, distances+1):
    data = []
    data_fractions = []
    csv_print("a", "b", "c", log=data)
    csv_print("a", "b", "c", log=data_fractions)
    for b in range(n+1):
        if b <= k:
            continue
        c = (1/2) * (math.pow(b, 2) / k + k)
        a = (1/2) * (math.pow(b, 2) / k - k)
        is_triple = math.pow(c, 2) == math.pow(a, 2) + math.pow(b, 2)
        csv_print(a, b, c, log=data)
        csv_print(Fraction(a), Fraction(b), Fraction(c), log=data_fractions)

    with open(f"pythagorean-triple-csv-generator_{k}.csv", 'w') as csvfile:
        csvfile.write('\n'.join(data))

    # I do this to handle issues with precision handling but it's not great in python
    # reason: is_triple = math.pow(c, 2) == math.pow(a, 2) + math.pow(b, 2)
    # is_triple will show false due to precision mismatch on the power and not be correct
    # I have checked and the values are fine, this will be my suffering for using python, lol
    with open(f"pythagorean-triple-csv-generator_fractions_{k}.csv", 'w') as csvfile:
        csvfile.write('\n'.join(data_fractions))
```

As you will see in the table, we now have a functioning way to calculate pythagorean triples for any number {%raw%}$$b$${%endraw%} within our natural number space.
<table>
  {% for row in site.data.pythagorean_triples_k_1 %}
    {% if forloop.first %}
    <tr>
      {% for pair in row %}
        <th>{{ pair[0] }}</th>
      {% endfor %}
    </tr>
    {% endif %}

    {% tablerow pair in row %}
      {{ pair[1] }}
    {% endtablerow %}
{% endfor %}
</table>

In this example, you can realise that only the odd values have whole pythagorean triples with the even values of {%raw%}$$b$${%endraw%} violating our natural number space requirements.
In the next sample, we will work through variants of the established pythagorean distance vector {%raw%}$$k$${%endraw%} in fixing these cases.


# Extending our pythagorean distance
In our previous example, we have established the formula works and we will arrive at pythagorean triples with a distance of 1 established by {%raw%}$$k = c - a$${%endraw%}. See the table above to validate this.

However, we have even numbers that do not express their triples as whole numbers. This can be fixed by increasing our pythagorean distance vector {%raw%}$$k$${%endraw%} to 2.


Let us start again from our factorization of 

{% raw %}
$$b^2 = (c + a) (c - a) $$
{% endraw %}

Let's take the factors to be fractional in this case where they are divided and increased by 2.
{% raw %}
$$c + a = \frac{b^2}{2} $$

$$c - a = 2$$
{% endraw %}

Solving for c gives the following outcome for the simultaneous equation:
{% raw %}
$$c + a = \frac{b^2}{2}$$

$$c - a = 2$$

--------------

$$2* c = \frac{b^2}{2} + 2$$
{% endraw %}

We arrive finally at
{% raw %}
$$c = \frac{1}{2} (\frac{b^2}{2} + 2)$$
{% endraw %}

Solving for {%raw%}a{%endraw%} also gives
{% raw %}
$$a = \frac{1}{2} (\frac{b^2}{2} - 2)$$
{% endraw %}


Using our fancy script, we regenerate the values for our table and we now have the opposite case with even values giving whole number and odd values giving fractional numbers.
<table>
  {% for row in site.data.pythagorean_triples_k_2 %}
    {% if forloop.first %}
    <tr>
      {% for pair in row %}
        <th>{{ pair[0] }}</th>
      {% endfor %}
    </tr>
    {% endif %}

    {% tablerow pair in row %}
      {{ pair[1] }}
    {% endtablerow %}
{% endfor %}
</table>

We also notice that in this even case, the odd sample for 2 in the earlier table `1.5,2,2.5` when multiplied by 2 gives us the even values `3,4,5` for the now established whole value of 2 in the even table.
This is an occurrence we will discuss later in this blog, pythagorean triples can be commutative where multiples of the linear values also correlate to the outcome of the quadratic expressions.

# Establishing finding pythagorean triples with pythagorean distances
When thinking about pythagorean triples, we usually have a hard time figuring out a pattern. However, this simplified when you consider there is a trick to it.

With the single value {%raw%}$$b$${%endraw%} which can exist with any distance from {%raw%}$$a$${%endraw%} and {%raw%}$$c$${%endraw%}, {%raw%}$$a$${%endraw%} and {%raw%}$$c$${%endraw%} will always have a fixed distance from each other.

With this, you arrive at a conclusion where the triples can be established with some pythagorean distance {%raw%}$$k$${%endraw%} such that all the members of this threesome satisfy each other (lol!)

{%raw%}
\\[ \forall b \in {\displaystyle \mathbb {N} }, \exists \;a \;,\; c \in {\displaystyle \mathbb {R} } \;forming \;a \;pythagorean \;triple \;\ni \; k \in {\displaystyle \mathbb {N} } \; \\{k = c - a \\} \\]



\\[ \therefore c = \frac{1}{2} (\frac{b^2}{k} + k) \\]

\\[ \therefore a = \frac{1}{2} (\frac{b^2}{k} - k) \\]
{%endraw%}

The expression above forms the universal pythagorean triple calculator based on some distance, this therefore implies that a single value {%raw%}$$b$${%endraw%} can produce multiple pythagorean triples dependent on what distance you apply. 

Taking {%raw%}$$k = 3$${%endraw%} as we have done for 1  and 2, we arrive at the same outcome. 9 for example produces 12 and 15 here whilst it produced 40 and 41 in the table for k=1.
<table>
  {% for row in site.data.pythagorean_triples_k_3 %}
    {% if forloop.first %}
    <tr>
      {% for pair in row %}
        <th>{{ pair[0] }}</th>
      {% endfor %}
    </tr>
    {% endif %}

    {% tablerow pair in row %}
      {{ pair[1] }}
    {% endtablerow %}
{% endfor %}
</table>

From here, we have established a formula to calculate any pythagorean triple within the real number space, however satisfying that the numbers are in the natural number space is experimental and I cover my approach to defining a boundary at the end of this article.

# Multiples of pythagorean triples
Within the same pythagorean distance {%raw%}$$k$${%endraw%}, you will find that several series of digits repeat. This means that pythagorean triples repeat within different sets as multiples.

For example, within {%raw%}$$k=4$${%endraw%}, we get the following sequence
<table>
  {% for row in site.data.pythagorean_triples_k_4 %}
    {% if forloop.first %}
    <tr>
      {% for pair in row %}
        <th>{{ pair[0] }}</th>
      {% endfor %}
    </tr>
    {% endif %}

    {% tablerow pair in row %}
      {{ pair[1] }}
    {% endtablerow %}
{% endfor %}
</table>

Within this sequence, we can use this value:
```
2.5,6,6.5
```

If we multiply this value by 2 to remove the fractions, we will get another pythagorean triple
```
5,12,13
```

However, you will notice 12 has another set of pythagorean triples different from what we would expect
```
16,12,20
```

When pythagorean triples run, you can have multiple sets within the natural number space with different pythagorean distances. In this case, both examples have {%raw%}$$k$${%endraw%} with values of 8 and 4 respectively when {%raw%}$$b=12$${%endraw%}.

Taking another from {%raw%}$$k=2$${%endraw%} and multiplying it by 2 for example gives one example on the table
```angular2html
3,4,5 -> 6,8,10
```

What you will find in examining pythagorean triples using the pythagorean distance approach is that triples will repeat and they are multiplicative reflections of each other.
Some will exist within the same distance if the distance repeats or extend into other versions of {%raw%}$$k$${%endraw%}.

# Prime behaviour for deriving original pythagorean triples
In succession to the chapter on repeating triples, you will find that getting new pythagorean triples is a matter of computing prime versions of {%raw%}$$b$${%endraw%} and establishing multiples of the related triple combinations.

All of this can be automated using the [Sieve of Eratosthenes](https://www.geeksforgeeks.org/sieve-of-eratosthenes/) or better yet, [Prime factorization using the square root method](https://byjus.com/maths/square-root-prime-factorization)


# Weird patterns in getting whole triples
To establish whole triples, one must arrive a case where {%raw%}$$c$${%endraw%} and {%raw%}$$a$${%endraw%} are both odd or even.

This therefore means that adding both values must result in a positive value before the addition of k.

We have established that:
{%raw%}$$c + a = \frac{b^2}{k}$${%endraw%}

Therefore, to arrive at whole triples, {%raw%}$$\frac{b^2}{k}$${%endraw%} must be even if k is even or odd if k is odd.

The table below where {%raw%}$$k=5$${%endraw%} showcases this summary
<table>
  {% for row in site.data.pythagorean_triples_k_5_natural %}
    {% if forloop.first %}
    <tr>
      {% for pair in row %}
        <th>{{ pair[0] }}</th>
      {% endfor %}
    </tr>
    {% endif %}

    {% tablerow pair in row %}
      {{ pair[1] }}
    {% endtablerow %}
{% endfor %}
</table>

# Conclusion
Decade long interests in math can have long-lasting benefits such as sleep deprivation and hyper-focus during these kinds of nostalgic moments.

I found writing this exciting and I hope you all as my "Dearest Gentle Readers" found this gossip amusing.

If you enjoyed this, I write about math problems I remember from day to day, one interesting being this optimisation case for [Project Euler, Problem 25](https://medium.com/an-idea/solving-project-euler-problem-25-4318b8df8bf7). Do send me those you also come across day-to-day.

Code for the simulations done for this article can be found here: [https://github.com/tiemma/unravelling-mathematical-threesomes](https://github.com/tiemma/unravelling-mathematical-threesomes) and a summary of mathematical symbols used in this article can be found here: [https://en.wikipedia.org/wiki/Glossary_of_mathematical_symbols](https://en.wikipedia.org/wiki/Glossary_of_mathematical_symbols )


<style>
img {
    min-height: unset !important;
    width: unset !important;
}
</style>

