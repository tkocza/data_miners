---
title: Distance metrics
date: 2020-02-29 00:00:00 +0100
categories: [Blogging, 'Machine Learning']
tags: ['Machine Learning', Metrics]
toc: false
usemathjax: true
seo:
  date_modified: 2020-03-01 22:00:50 +0100
---

In this post I would like to say something more about **distace metrics**, which are necessary to resolve Machine Learning problems.
If you want to understand how yours models works you cannot skip the distances.
 
## **Minkowski distance**
When you try to image that you need to cover a distance you will say e.g. "I have to travel 10 kilometers". You will never say "-10 kilometers". It explains how we perceive a distance. this also the base for Minkowski distance theory.
The main assumption of this method is that distance always has to be positive value.

$$ D(p,q) = \left( \sum_{i=1}^N |p_i - q_i|^k \right)^ \frac{1}{k} $$

Regarding to *p* value we can transform this formula as:
- *k = 1* - Manhattan metric
- *k = 2* - Euclidean metric
- *k = ∞* - Chebyshev metric
 
## **Euclidean distance**
The Euclidean metric is the most often use distance in Machine Learning. It is simple straight line between two point in *n* dimensions space. 
You can also find this distance as Pythagorean metric.

$$ D(p,q) = \sqrt{(p_1-q_1)^2 + (p_2-q_2)^2 + ... + (p_n-q_n)^2} $$

$$ = $$

$$ \sqrt{\sum_{i=1}^N (p_i-q_i)^2} $$

## **Manhattan distance**
This distance is also known as *Taxicab distance* or *Rectilinear distance*. The name comes from Manhattan streets grid layout. 
The distance is calculated as absolute sum of the difference between its cartesian coordinates.

$$ D(p,q) = |p_1 - q_1| + |p_2 - q_2| + ... + |p_n - q_n| $$ 

$$ = $$

$$ \sum_{i=1}^N |p_i - q_i| $$

## **Chebyshev distance**
The metric is calculated as maximum value of absolute difference between point in compared vectors. That is the reason why it is also called *Maximum metric*.
Other name which you can see is *Chessboard distance* because represent distance between two spaces on a chess board gives the minimum number of moves a king requires to move between them.

$$ D(p,q) :=  max \{|p_1 - q_1|, |p_2 - q_2|, ... |p_n - q_n|\} $$ 

$$ = $$ 

$$ \max_{i} \{|p_i - q_i|\} $$

Let's count a distance in the order to above different methods.
You have two cases:

1) two dimensions points (2D):
- A (2,4)
- B (6,7)
    
2) three dimensions points (3D):
- C (2,4,6)
- D (6,7,12)

{:class="table table-bordered"}
| Metric        | 2D Formula | 2D result | 3D Formula | 3D result |
|:-------------:|:-------------:|:-------------:|:---------:|:---------:|
| Euclidean     | $$ \sqrt{(2-6)^2 + (4-7)^2} $$ | 5 | $$ \sqrt{(2-6)^2 + (4-7)^2 + (6-12)^2} $$ | 7.81 |
| Manhattan     | $$ \|2-6\| + \|4-7\| $$ | 7 | $$ \|2-6\| + \|4-7\| + \|6-12\| $$  | 13 |
| Chebyshev     | $$ max\{\|2-6\|,\|4-7\|\} $$ | 4 | $$ max\{\|2-6\|,\|4-7\|,\|6-12\|\} $$ | 6 |

## **Cosine similarity**
The distance describe the cosine value of the angle between two vectors. 
Two vectors with the same orientation have a cosine similarity of 1, two vectors oriented at 90° relative to each other have a similarity of 0, and two vectors diametrically opposed have a similarity of -1.
This method is useful for finding similarity for values with quite big range e.g. in text analysis for number of words.

$$ \cos\theta = \frac{\sum_{i=1}^N a_i b_i}{\sqrt{\sum_{i=1}^N a_i^2} \sqrt{\sum_{i=1}^N b_i^2} } $$

## **Hamming distance**
This metric shows the number of differences in given (usually binary) vectors with the same length.
The most often usage is text analysis but also it can be applied in other cases like e.g. image similarity and genetics analysis.   

Examples:
- "0**1**11**0**01" and "0**0**11**1**01" the distance is **2**
- "12**3**4**5****6**7" and "12**2**3**3****2**67" the distance is **3**
- "l**a**ke" and "l**i**ke" the distance is **1**
- "A**T**TGCT**A**" and "A**G**TGCT**C**"  the distance is **2**
 
## **Jaccard distance**
The Jaccard coefficient measures similarity and diversity of sample sets. 
There is not matter the content of sets, they could be numeric or text values.

$$ JaccardIndex(A,B) = \frac{ |A \cap B| }{ |A \cup B| } $$

$$ = $$

$$ \frac{ |A \cap B| }{ |A| + |B| - |A \cap B| } $$

$$ JaccardDistance = 1 - JaccardIndex $$

*Example:*

There are two sets:

A = [10, 12, 14, 15]

B = [12, 13, 15, 16, 17, 18]

Common part of those are [12, 15] so 2 elements.

The sum of all elements in sets is 20 (4 + 6)

The final calculation:

$$ JaccardIndex = \frac{2}{4 + 4 - 2} = 0.33 $$

$$ JaccardDistance = 0.67 $$


## **Levenshtein distance**
This metric shows how many changes has to be done in one vector to achieve second vector.
There are 3 kinds of changes:

1) insertion

2) substitution

3) deletion

This method is usually used in spell checkers, natural language processing and optical character recognition but of course the usage can be adjusted to needs.

*Example:*

Try to compare two words: "SLEEP" and "KEEP"

{:class="table table-bordered"}
|:---:|:---:|:---:|:---:|:---:|
| **S** | **L** | E | E | P | 
| **-** | **K** | E | E | P |

If you want to turn "sleep" into "keep" you need to do two changes: one insertion and one substitution.

The Levenshtein distance for this 2.  

<br/>

### Thanks for reading! 