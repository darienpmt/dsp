[Think Stats Chapter 5 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2006.html#toc50) (blue men)

# Think Stats Exercise 5.1
The purpose of this exercise is to compute the probability that a randomly selected man from the United States is eligible to be a part of the "Blue Man Group." In order to be eligible, you need to be between 5'10" and 6'1". Using data from the Behavioral Risk Factor Surveillance System (BRFSS), we were able to determine that the heights of men are roughly normally distributed with a mean of 178 cm and standard deviation of 7.7 cm. Since we are invesitgating this question for the entire population of men, we will not use percentiles from the BRFSS data set, but rather use the analytic form of the normal distribution to determine the probability.

## 1. Probability that a man from the U.S. is between 5'10" and 6'1"
In order to answer this question we will use the analytic normal distribution from scipy.stats, giving it the required parameters from the BRFSS data set.
```python3
import scipy.stats
mu = 178
sigma = 7.7
dist = scipy.stats.norm(loc=mu, scale=sigma)
```
`dist` is what's called a 'frozen random variable.' This means that this analytic normal distribution is defined by the parameters we've given it (this is so that we do not have to provide the `loc` and `sigma` parameters each time we want to do something with it). For example, we can compute the mean and standard deviation of `dist`:
```python3
dist.mean(), dist.std()
```
Which, unsurprisingly, gives us `178 cm` and `7.7 cm` respectively. `scipy.stats.norm` also has a `cdf()` method. We can verify that this works by checking the probability that man is less than `178 cm` is `0.5` (since we are assuming distribution is normal, half of the men should be above and below the mean).
```python3
dist.cdf(178)
```
Which does in fact give us `0.5`.
So now we are finally ready to answer the question. There may be a more "slick" way of solving this problem, but I will just convert the American feet and inches notation to centimeters (using Google's 1700 engineers), then compute the difference between `dist.cdf(185.4)` and `dist.cdf(177.7)`. This will give us the probability that a man is between `185.4 cm` and `177.8 cm` because `dist.cdf(185.4)` on its own tells us the probability that a man is below `185.4 cm`, so we just need to take away any chance of the man being less than `177.8 cm`.
```python3
dist.cdf(185.4) - dist.cdf(177.8)
```
Which tells us the the probability of a man being eligible for Blue Man Group is `0.343`, or `34.3%`. This should be no surprise since `185.4` is about one standard deviation away from the mean `(178 + 7.7)`, where about 34% of data values in any normally distributed data set lie.


