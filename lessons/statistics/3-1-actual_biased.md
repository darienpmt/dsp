[Think Stats Chapter 3 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2004.html#toc31) (actual vs. biased)

# Think Stats Exercise 3.1
The purpose of this exercise is to investigate the appearance of a "class size paradox" if you were to survey children and ask them how many children there are in their family. We will again use data from NSFG, this time looking at the number of children under the age of 18 in each family. 

## 1. The paradox

The paradox applies in this context because children with more siblings are more likely to:

- get chosen in a random sample, and therefore
- report a large number of children in their family

This will skew the actual mean number of children in a collection of families. The **basic** logic is as follows. Say you have an absurdly small amount of families and they have 1, 1 and 4 children respectively. If your random sample contains one child from each family, you will happen to get an accurate result for the mean number of childen (2 per family). Since 2/3 of the "population" comes from a family with 4 children, they are more likely to get chosen for the sample, and thus raise the true mean number of children. Therefore there is bias in the sample.

## 2. Actual vs. Bias PMFs
We first load the data from the FemResp dataset:
```python3
resp = nsfg.ReadFemResp()
```
From this DataFrame, we create two probability mass functions: one that will give the actual mean number of childen, and another that is biased:

```python3
numkdhh_pmf = thinkstats2.Pmf(resp.numkdhh, label='actual')
biased_pmf = BiasPmf(numkdhh_pmf, label='biased')
```
Where `numkdhh` is the name of the column in `resp` which contains the data we need. How is the "biased" PMF calculated? 
As seen in the discussion above, a child from a family of four is 4 times more likely to get surveyed than an on only child. Therefore the bias is depended on the number of children. To determine the biased PMF, we simply modify the actual PFM (`numkdhh_pmf`) by multiplying each probability in the PMF by it's corresponding random variable. The python code to produce `biased_pmf` is shown below:

```python3
def BiasPmf(pmf, label):
    new_pmf = pmf.Copy(label=label)

    for x, p in pmf.Items(): # loops through the number of children and their probabilities
        new_pmf.Mult(x, x) # multiplies each probability by the number of children in that family
        
    new_pmf.Normalize() # ensures the sum of the probabilities will be 1, thus making new_pmf an actual PMF
    return new_pmf
   ```
 ## 3. Comparison of means
Once we have each of our necessary probability mass functions, the comparison is actually quite easy. First, lets plot each distribution on the same set of axis to see how they compare generally. To do this, we run the following code:

```python3
thinkplot.Pmfs([numkdhh_pmf, biased_pmf])
thinkplot.Config(xlabel='num_kids', ylabel='probability')
```
This code produces the following figure:
 
![](https://github.com/darienpmt/dsp/blob/master/lessons/statistics/ex_0301_plot.png)

It's clear from the figure that the biased PMF shows there being more families with a greater number of children than in the actual PMF. This is not a surprising result. We calculate the mean of each PMF to get a concrete comparison of their central tendencies.

```python3
numkdhh_pmf.Mean()
biased_pmf.Mean()
```

The actual mean number of children in a family is about `1.02` whereas the bias mean is `2.40`, more than double the actual mean. This shows the effect of the 'class size paradox' when surverying children about how many kids there are in their family is quite large.

