---
layout: post
title:  "Introduction to probabilities (Breen's class)"
categories: [jekyll, rstats]
tags: [knitr, servr, httpuv, websocket]
---

<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>


## Some concepts 

Number of outcomes 

Sample space $$\{1, 2, 3, 4\}$$ 

Probabilities of events 

$$ P \{1, 2\} = \frac{1}{3} $$ 

### Disjunction 

Events are not duplicated. All the probabilities sum up to 1. 

$$P \{5, 6\}$$

$$P \{1\}$$

$$P \{2,3,4\}$$

### Joint Events 

$$A = \{1,2\}$$

$$B = \{2,4\}$$

$$P(A) = \frac{1}{3} $$

$$P(B) = \frac{1}{3} $$

In set term : Union. 

$$ P(A \cup B) = P(A) + P(B) - P(A \cap B) $$

$$ P(A \cup B) = \frac{1}{3} + \frac{1}{3} - \frac{1}{6} = \frac{1}{2} $$

### Cards 

52 cards 

$$P(Ace) = \frac{4}{52} $$

$$P(Red) = \frac{26}{52} $$

$$ P(A \cup R) = P(A) + P(R) - P(A \cap R) $$

$$ P(A \cup R) = \frac{4}{52} + \frac{26}{52} - \frac{2}{52} = \frac{1}{...} $$

## Multiplication of probabilities 

Ask yourself if the observations are *independant* or not. If Event influence the probabilities of one another ("Associated"). 

$$ X == red $$ 

$$ Y == queen $$ 

What is the probability of taking a red queen (X,Y) ? 

$$ P(X, Y) = P(X) \times P(Y)$$ 

$$ P(X, Y) = \frac{26}{52} \times \frac{4}{52}$$ 

#### Example 

$$A = \{1,2\}$$

$$B = \{2,4\}$$

$$C = \{1,2,3,5,6\}$$

$$ P(A \cup B \cup C) = P(A) + P(B) + P(C) - P(A \cap B) - P(A \cap C) - P(B \cap C) + P(A \cap B \cap C) $$


### Non independance

$$ X \perp Y $$ 

What is the probabilities of having 2 aces ? 

1. With replacement 

$$ P(Ace = 2) = \frac{4}{52} \times \frac{4}{52} $$

2. Without replacement 

$$ P(Ace = 2) = \frac{4}{52} \times \frac{3}{52} $$

#### Example 

<table style="border-collapse:collapse;border-spacing:0;border-color:#ccc"><tr><th style="font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:#ccc;color:#333;background-color:#f0f0f0;text-align:center"><br></th><th style="font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:#ccc;color:#333;background-color:#f0f0f0;text-align:center">Labour</th><th style="font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:#ccc;color:#333;background-color:#f0f0f0;text-align:center">Conservatice</th><th style="font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:#ccc;color:#333;background-color:#f0f0f0;text-align:center;vertical-align:top"></th></tr><tr><td style="font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:#ccc;color:#333;background-color:#fff;text-align:center;vertical-align:top">Men </td><td style="font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:#ccc;color:#333;background-color:#fff;text-align:center;vertical-align:top">5</td><td style="font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:#ccc;color:#333;background-color:#fff;text-align:center;vertical-align:top">12</td><td style="font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:#ccc;color:#333;background-color:#fff;text-align:center;vertical-align:top">17</td></tr><tr><td style="font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:#ccc;color:#333;background-color:#fff;text-align:center;vertical-align:top">Women</td><td style="font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:#ccc;color:#333;background-color:#fff;text-align:center;vertical-align:top">33</td><td style="font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:#ccc;color:#333;background-color:#fff;text-align:center;vertical-align:top">10</td><td style="font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:#ccc;color:#333;background-color:#fff;text-align:center;vertical-align:top">43</td></tr><tr><td style="font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:#ccc;color:#333;background-color:#fff;text-align:center;vertical-align:top"></td><td style="font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:#ccc;color:#333;background-color:#fff;text-align:center;vertical-align:top">38</td><td style="font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:#ccc;color:#333;background-color:#fff;text-align:center;vertical-align:top">22</td><td style="font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:#ccc;color:#333;background-color:#fff;text-align:center;vertical-align:top">60<br></td></tr></table>


** Marginal Unconditional Probability ** 

$$ P(X) $$

$$ P(sex == men) = \frac{17}{60} $$

** Join Bivariate Proability ** 

$$ P(X, Y) $$

$$ P(sex == men \& party == labour) = \frac{5}{60} $$

** Conditional Probability ** 

$$ P(Y \mid X) $$

$$P(Labour \mid Men)  = \frac{5}{17}$$

Men is a *fixed condition* 

##### Conditional Probability 

$$ P(Y \mid X) = \frac{P(X,Y)}{P(X)}$$

*theoretical distribution independant* 

$$ P(X, Y) =  P(X) \times P(Y) $$ 

$$ X \perp Y = P(Y \mid X) = \frac{P(X,Y)}{P(X)} = \frac{P(X) \times P(Y)}{P(X)} = P(Y) $$ 

> "independance : knowing one thing does not inform about another thing"  

## Law of Total Probability 

- Disjunction events : $$ A_{1}, A_{2}, \dots, A_{n}$$ 

$$ P(B) = P(B|A_{1}) \times P(A_{1}) + \dots $$

##### Example 

10 players 

- 3 goods (G) $$ P(Win = 0.2) $$ 
- 5 indifferent (I) $$ P(Win = 0.5) $$ 
- 2 bads (B) $$ P(Win = 0.8) $$ 

What is the probability of winning the first match ? 

$$ P(win) = P(w \mid G) \times P(G) + P(w \mid I) \times P(I) + P(w \mid B) \times P(B) = 
(0.2 \times 0.3) + (0.5 \times 0.5) + (0.2 \times 0.8) $$ 

## Bayes Theorem 

$$ P(Y \mid X) = \frac{P(X \mid Y) \times P(Y)}{P(X)}

Changing your belief after observations 

Frequentists (all in data) versus bayesian (data + update)

#### Example 

Disease (D) and the test detection (T) 

- Proba of catching the disease

$$ P(D = 1) = \frac{1}{10`000} $$ 

$$ Test = 
  \begin{cases}
    1      & \quad \text{if } T \text{ is positive}\\
    0  & \quad \text{if } T \text{ is negative}\\
  \end{cases}
$$ 


$$ P(T = 1 \mid D = 1) = 0.999 $$ 

*test positif but no disease* 
$$ P(T = 1 \mid D = 0) = 0.0001 $$ 

$$ P(T = 1 \mid D = 1) = \frac{ P(D = 1 \mid T = 1 ) \times P(D = 1) }{ P ( T = 1 ) } = \frac{1}{2} $$  

$$ \frac{0.999 \times 0.0001}{T = 1} $$ 

What is $$ T = 1 $$ ? Use the Law Total Probability 

$$ P(T = 1) = P(T = 1 \mid D = 1) \times P(D = 1) + \frac{P(T = 1 | D = 0)}{P(D = 0)} $$ 

Take again the test and replace $$ D = 1 $$ of $$ 0.0001 $$ but with replace with the new posterior beliefs $$ 0.5 $$ 








