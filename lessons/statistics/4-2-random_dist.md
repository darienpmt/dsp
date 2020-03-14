[Think Stats Chapter 4 Exercise 2](http://greenteapress.com/thinkstats2/html/thinkstats2005.html#toc41) (a random distribution)

# Think Stats Exercise 4.2
Numpy has a random number generator np.random.random. It generates each number with equal probability, meaning the distribution of numbers should be uniform. The purpose of this exercise is to investigate whether or not this is true by generating 1000 numbers randomly and plotting thier PMF and CDF. We would expect the PMF to show us that each random variable has the same probability. The CDF should be close to a linear function in appearance since each successive accumulation of probability should be the same as the previous (because each random variable has the same probability of occuring).

## 1. Visualizing the PMF
To visualize the PMF we need to use numpy's random.random function, then use built in thinkstats2 functions for the PMF and the visual. For both the PMF and the CDF we will use 1000 randomly generated numbers between 0 and 1.

```python3
rand_1000 = np.random.random(1000)
```
Then the following generates a PMF from the given array and plots the PMF:

```python3
random1000_pmf = thinkstats2.Pmf(rand_1000)
thinkplot.Pmf(random1000_pmf)
thinkplot.Config(xlabel='Random Outcome', ylabel='Probability')
```

[image will go here, need to figure this out]

That is not very helpful. It's clear that each random variable's correspoding probability is around the same value (about 0.001, which makes sense) but the visual just looks like a blob. This is because there have been so many numbers generated, and they are all so close together, we cannot distinguish between different outcomes. To see what's going on here a bit more clearly, let's conduct the same process as above but for only 10 and 100 randomly generated numbers.

```python3
rand_10 = np.random.random(10)
rand_100 = np.random.random(100)
rand_1000 = np.random.random(1000)

thinkplot.PrePlot(3, cols=3)

random10_pmf = thinkstats2.Pmf(rand_10)
thinkplot.Pmf(random10_pmf, linewidth=0.8)
thinkplot.Config(ylabel='Probability', axis=[0.0, 1.0, 0.0, 0.11] )

thinkplot.SubPlot(2)
random100_pmf = thinkstats2.Pmf(rand_100)
thinkplot.Pmf(random100_pmf, linewidth=0.5)
thinkplot.Config(xlabel='Random Outcome', axis=[0.0, 1.0, 0.0, 0.011])

thinkplot.SubPlot(3)
random1000_pmf = thinkstats2.Pmf(rand_1000)
thinkplot.Pmf(random1000_pmf, linewidth=0.1)
thinkplot.Config(axis=[0.0, 1.0, 0.0, 0.0011])
```
[image will go here, need to figure this out]

For the visuals with only 10 and 100 randomly generated numbers respectively, we can more easily see the distinct outcomes, with each of them occuring with the same probability (0.1 and 0.01 respectively). For our 1000 randomly generated numbers, I made the lines on the graph thinner so that we can more easily see distictions. A PMF may not be the best way to visualize this data, but it does confirm that np.random.random is uniform between 0 and 1.

# 2. Visualizing the CDF
We build the visual for the CDF in a similar fassion:
```python3
random_cmf = thinkstats2.Cdf(rand_1000)
thinkplot.Cdf(random_cmf)
thinkplot.Config(xlabel='Random Outcome', ylabel='Cumulative Prob')
```
[image will go here, need to figure this out]

The CDF is approximatedly a straight line, which, as stated above, shows that np.random.random generates a uniform distribution of numbers.
