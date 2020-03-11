[Think Stats Chapter 2 Exercise 4](http://greenteapress.com/thinkstats2/html/thinkstats2003.html#toc24) (Cohen's d)

## Think Stats Exercise 2.4

The goal of this problem is to investigate whether first born babies are lighter or heavier in weight as compared to others. We will then make a comparison with answering the same question but with pregancy lengths. The data set is provided by the National Survey of Family Growth, via Allen Downey's Github repository for Think Stats.

### 1. Compare means

A natural place to start would be to compare the mean weights of first born babies versus others. In order to do this, we need to ensure that we have the appropriate data. First, we need to load in the data from the pregnancy file and filter the data set only looking at live births:

```python3
preg = nsfg.ReadFemPreg()
live = preg[preg.outcome == 1]
```
We can then further filter the data, separting it into two DataFrame's - one looking at first born babies, and one looking at others.
```python3
firsts = live[live.birthord == 1]
others = live[live.birthord != 1]
```
We are now all set to look at the difference in mean weights of first born babies versus the weights of others:
```python3
mean_diff = firsts.totalwgt_lb.mean() - others.totalwgt_lb.mean() 
```
Which returns a value of ```-0.12476118453549034```. This tells us that first born babies are `0.125 pounds`, or around `2 ounces`, lighter than babies who are not first born. As a portion of the typical newborn weight, this is a `1.6%` difference. Although this is small, it is `8` times larger than the difference in mean pregancy length for first born babies versus others. 

### 2. Cohen's d statistic
Another way to desribe the difference between first born weight's and non-first born weights are through Cohen's d statistic. Cohen's d statistic may be more effective than comparing means outright because it allows us to look at a standardized difference between two means (meaning it is a statistic measured in standard deviation units). It is computed in the following way:

![d = \frac{\bar{x}_1 - \bar{x}_2}{s}](https://render.githubusercontent.com/render/math?math=d%20%3D%20%5Cfrac%7B%5Cbar%7Bx%7D_1%20-%20%5Cbar%7Bx%7D_2%7D%7Bs%7D)

Where `s` is the pooled standard deviation (a weighted average of the standard deviations of multiple groups with more weight given to groups with more data values). 

We use the following function in order to calculate Cohen's d statistic for two Series (or two DataFrames):
```python3
def CohenEffectSize(group1, group2):
    """Computes Cohen's effect size for two groups.
    
    group1: Series or DataFrame
    group2: Series or DataFrame
    
    returns: float if the arguments are Series;
             Series if the arguments are DataFrames
    """
    diff = group1.mean() - group2.mean()

    var1 = group1.var()
    var2 = group2.var()
    n1, n2 = len(group1), len(group2)

    pooled_var = (n1 * var1 + n2 * var2) / (n1 + n2)
    d = diff / np.sqrt(pooled_var)

    return d
   ```
   To calculate cohen's d for total weight of first babies versus others:
   ```python3
   cohend_wgt = CohenEffectSize(firsts.totalwgt_lb, others.totalwgt_lb)
   ```
Which returns a value of `-0.08867292707260174`, telling us the difference in means is about 0.0887 standard deviations. Comparing this the difference in pregancy lengths, which has a cohen's d of `0.0289`, we see that the difference is about `3` times larger when looking at weight as compared to pregancy lenght.
