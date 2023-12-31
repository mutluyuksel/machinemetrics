
# Bias-Variance Trade-off 

We already discuss bias and variance of estimator, and decomposition of MSE for estimation above in MSE section. Now, Lets discuss what is bias and variance and trade-off between them in predictive models. To remind,our task is prediction of an outcome, Y using the data (more accurately test/train data) .

To "predict" Y using features X, means to find some $f$ which is close to $Y$. We assume that $Y$ is some function of $X$ plus some random noise.

$$Y=f(X)+ ϵ$$
However, we can never know real $f(X)$. Thus our goal becomes to finding some $\hat{f(X)}$ that is a good estimate of the regression function $f(X)$. There will be always difference between real $f(X)$ and $\hat{f(X)}$. That difference is called reducible error. We find a good estimate of $f(X)$ by reducing the expected mean square of error of test data as much as possible. Thus, we can write 

**MSE for prediction function using training data** (for test data, validation sample):

$$
\operatorname{MSE}(f(x), \hat{f}(x))= \underbrace{(\mathbb{E}[\hat{f}(x)]-f(x))^2}_{\operatorname{bias}^2(\hat{f}(x))}+\underbrace{\mathbb{E}\left[(\hat{f}(x)-\mathbb{E}[\hat{f}(x)])^2\right]}_{\operatorname{var}(\hat{f}(x))}
$$

Then,  

**Mean Square Prediction Error** can be written as:

$$
\mathbf{MSPE}=\mathbf{E}\left[(Y-\hat{f(X)})^{2}\right]=\mathbf{Bias}[\hat{f(X)}]^{2}+\mathbf{Var}[\hat{f(X)}]+\sigma^{2}
$$
$\sigma^{2}=E[\varepsilon^{2}]$

Expected mean-squared prediction error (MSPE) on the validation/training sample is sum of squared bias of the fit and variance of the fit and variance of the error.

**Variance** is the amount by which $\hat{f(X)}$ could change if we estimated it using different test/training data set. $\hat{f(X)}$ depends on the training data. (More complete notation would be $\hat{f}(X; Y_{train},X_{train})$ )If the $\hat{f(X)}$ is less complex/less flexible, then it is more likely to change if we use different samples to estimate it. However, if $\hat{f(X)}$ is more complex/more flexible function, then it is more likely to change between different test samples. 

For instance, Lets assume we have a data set (test or training data) and we want to "predict" Y using features X, thus estimate function $f(X)$. and lets assume we have 1-degree and 10-degree polynomial functions as a potential prediction functions. We say $\hat{f(X)}=\hat{\beta_{0}} + \hat{\beta_{1}} X$ is less complex than 10-degree polynomial $\hat{f(X)}=\hat{\beta_{0}} + \hat{\beta_{1}} X...+ \hat{\beta_{10}} X^{10}$ function. As, 10-degree polynomial function has more parameters $\beta_{0},..\beta_{10}$, it is more flexible. That also means it has high variance (As it has more parameters, all these parameters are more inclined to have different values in different training data sets). Thus, a prediction function has high variance if it can change substantially when we use different training samples to estimate $f(X)$. We can also say less flexible functions (functions with less parameters) have low variance. Low variance functions are less likely to change when we use different training sample or adding new data to the test sample. We will show all these with simulation in overfitting chapter as well.     

**Bias** is the difference between the real! prediction function and expected estimated function. If the $\hat{f(X)}$ is less flexible, then it is more likely to have higher bias. We can think this as real function(reality) is always more complex than the function approximates the reality. So, it is more prone to have higher error, i.e. more bias.  
In the context of regression, Parametric models are biased when the form of the model does not incorporate all the necessary variables, or the form of the relationship is too simple. For example, a parametric model assumes a linear relationship, but the true relationship is quadratic. In non-parametric models when the model provides too much smoothing.

There is a bias-variance tradeoff. That is, often, the more bias in our estimation, the lesser the variance. Similarly, less variance is often accompanied by more bias. Flexible(i.e. complex) models tend to be unbiased, but highly variable. Simple models are often extremely biased, but have low variance.

So for us, to select a model that appropriately balances the tradeoff between bias and variance, and thus minimizes the reducible error, we need to select a model of the appropriate flexibility for the data. However, this selected model should not overfit the data as well which we will discuss in the next section.

https://threadreaderapp.com/thread/1584515105374339073.html

https://www.simplilearn.com/tutorials/machine-learning-tutorial/bias-and-variance

Although conceptually the variance-bias trade-off seems intuitive, at least mathematically, we need to ask another practical question: Can we see the components of the decomposed MSPE? We may not see it in the actual data ( as we donot know real function and irreducible error) but we can show it with simulations.  

We will use the same example we worked with before.  We have years of schooling , which changes between 9 and 16.  We sample from this "population" multiple times. Now the task is to use each sample and come up with a predictor (a prediction rule) to predict a number or multiple numbers drawn from the same population.


```r
# Here is our population
populationX <- c(9,10,11,12,13,14,15,16)


#Let's have a containers to have repeated samples (2000)
Ms <- 5000
samples <- matrix(0, Ms, 10)
colnames(samples) <- c("X1", "X2", "X3", "X4", "X5", "X6", "X7", "X8", "X9", "X10")

# Let's have samples (with replacement always)
set.seed(123)
for (i in 1:nrow(samples)) {
  samples[i,] <- sample(populationX, 10, replace = TRUE)
}
head(samples)
```

```
##      X1 X2 X3 X4 X5 X6 X7 X8 X9 X10
## [1,] 15 15 11 14 11 10 10 14 11  13
## [2,] 12 14 14  9 10 11 16 13 11  11
## [3,]  9 12  9  9 13 11 16 10 15  10
## [4,]  9 14 11 12 14  9 11 15 13  12
## [5,] 15 16 10 13 15  9  9 10 15  11
## [6,] 12 13 15 13 11 16 14  9 10  13
```

Now, Let's use our predictors: 



```r
# Container to record all predictions
predictions <- matrix(0, Ms, 2)

# fhat_1 = 9
for (i in 1:Ms) {
  predictions[i,1] <- 10
}

# fhat_2 - mean
for (i in 1:Ms) {
  predictions[i,2] <- sum(samples[i,])/length(samples[i,])
}

head(predictions)
```

```
##      [,1] [,2]
## [1,]   10 12.4
## [2,]   10 12.1
## [3,]   10 11.4
## [4,]   10 12.0
## [5,]   10 12.3
## [6,]   10 12.6
```

Now let's have our MSPE decomposition:


```r
# MSPE
MSPE <- matrix(0, Ms, 2)
for (i in 1:Ms) {
  MSPE[i,1] <- mean((populationX-predictions[i,1])^2)
  MSPE[i,2] <- mean((populationX-predictions[i,2])^2)
}
head(MSPE)
```

```
##      [,1] [,2]
## [1,] 11.5 5.26
## [2,] 11.5 5.41
## [3,] 11.5 6.46
## [4,] 11.5 5.50
## [5,] 11.5 5.29
## [6,] 11.5 5.26
```

```r
# Bias
bias1 <- mean(populationX)-mean(predictions[,1])
bias2 <- mean(populationX)-mean(predictions[,2])

# Variance (predictor)
var1 <- var(predictions[,1])
var1
```

```
## [1] 0
```

```r
var2 <- var(predictions[,2])
var2
```

```
## [1] 0.5385286
```

```r
# Variance (epsilon)
var_eps <- mean((populationX-12.5)^2)
var_eps
```

```
## [1] 5.25
```

Let's put them in a table:


```r
VBtradeoff <- matrix(0, 2, 4)
rownames(VBtradeoff) <- c("fhat_1", "fhat_2")
colnames(VBtradeoff) <- c("Bias", "Var(fhat)", "Var(eps)", "MSPE")
VBtradeoff[1,1] <- bias1^2
VBtradeoff[2,1] <- bias2^2
VBtradeoff[1,2] <- var1
VBtradeoff[2,2] <- var2
VBtradeoff[1,3] <- var_eps
VBtradeoff[2,3] <- var_eps
VBtradeoff[1,4] <- mean(MSPE[,1])
VBtradeoff[2,4] <- mean(MSPE[,2])
round(VBtradeoff, 3)
```

```
##        Bias Var(fhat) Var(eps)   MSPE
## fhat_1 6.25     0.000     5.25 11.500
## fhat_2 0.00     0.539     5.25  5.788
```
  
This table clearly shows the decomposition of MSPE. The first column is the contribution to the MSPE from the bias, and the second column is the contribution from the variance of the predictor. These together make up the reducible error. The third column is the variance that comes from the data, the irreducible error. The last column is, of course, the total MSPE, and we can see that $\hat{f}_2$ is the better predictor because of its lower MSPE.  



## Biased estimator as a predictor

Upto this point, we showed in the simulation prediction function with zero bias but high variance produce better prediction than the prediction function with zero variance but high bias. However, we can obtain better prediction function which has some bias and some variance. Better prediction function means smaller MSPE. Thus if the decline in variance would be more than then the bias in the second prediction function, then we have better predictor.  
Lets show this with equation first and then with simulation.

We saw earlier that $\bar{X}$ is a better estimator. Now, Let's define a biased estimator of $\mu_x$:

$$
\hat{X}_{biased} = \hat{\mu}_x=\alpha \bar{X}
$$
  
The sample mean $\bar{X}$ is an unbiased estimator of $\mu_x$. The magnitude of the bias is $\alpha$ and as it goes to 1, the bias becomes zero.  As before, we are given one sample with three observations from the same distribution (population). We want to guess the value of a new data point from the same distribution.  We will make the prediction with the best predictor which has the minimum MSPE.   By using the same decomposition we can show that:  

$$
\hat{\mu}_x=\alpha \bar{X}
$$
  
$$
\mathbf{E}[\hat{\mu}_x]=\alpha \mu_x
$$
  
$$
\mathbf{MSPE}=[(1-\alpha) \mu_x]^{2}+\frac{1}{n} \alpha^{2} \sigma_{\varepsilon}^{2}+\sigma_{\varepsilon}^{2}
$$
  
Our first observation is that when $\alpha$ is one, the bias will be zero.  Since it seems that MSPE is a convex function of $\alpha$, we can search for $\alpha$ that minimizes MSPE.  The first-order-condition would give us the solution:

$$
\frac{\partial \mathbf{MSPE}}{\partial \alpha} =0 \rightarrow ~~ \alpha = \frac{\mu^2_x}{\mu^2_x+\sigma^2_\varepsilon/n}<1
$$

Using the same simulation sample above , lets calculate alpha and MSPE with this new biased prediction function, and compare all 3 MSPEs. 


```r
pred <-rep(0, Ms)

# The magnitude of bias
alpha <- (mean(populationX))^2/((mean(populationX)^2+var_eps/10))
alpha
```

```
## [1] 0.9966513
```

```r
# Biased predictor
for (i in 1:Ms) {
  pred[i] <- alpha*predictions[i,2]
}
# Check if E(alpha*Xbar) = alpha*mu_x
mean(pred)
```

```
## [1] 12.45708
```

```r
alpha*mean(populationX)
```

```
## [1] 12.45814
```

```r
# MSPE
MSPE_biased <- rep(0, Ms)
for (i in 1:Ms) {
  MSPE_biased[i] <- mean((populationX-pred[i])^2)
}
mean(MSPE_biased)
```

```
## [1] 5.786663
```
  
Let's add this predictor into our table:  


```r
VBtradeoff <- matrix(0, 3, 4)
rownames(VBtradeoff) <- c("fhat_1", "fhat_2", "fhat_3")
colnames(VBtradeoff) <- c("Bias", "Var(fhat)", "Var(eps)", "MSPE")
VBtradeoff[1,1] <- bias1^2
VBtradeoff[2,1] <- bias2^2
VBtradeoff[3,1] <- (mean(populationX)-mean(pred))^2
VBtradeoff[1,2] <- var1
VBtradeoff[2,2] <- var2
VBtradeoff[3,2] <- var(pred)
VBtradeoff[1,3] <- var_eps
VBtradeoff[2,3] <- var_eps
VBtradeoff[3,3] <- var_eps
VBtradeoff[1,4] <- mean(MSPE[,1])
VBtradeoff[2,4] <- mean(MSPE[,2])
VBtradeoff[3,4] <- mean(MSPE_biased)
round(VBtradeoff, 3)
```

```
##         Bias Var(fhat) Var(eps)   MSPE
## fhat_1 6.250     0.000     5.25 11.500
## fhat_2 0.000     0.539     5.25  5.788
## fhat_3 0.002     0.535     5.25  5.787
```

As seen , increase in bias is lower than decrease in variance. The prediction function with some bias and variance is the **best prediction function** as it has the smallest MSPE.
 This example shows the difference between estimation and prediction for a simplest predictor, the mean of $X$.  We will see a more complex example when we have a regression later.   

Another simulation examples are
https://blog.zenggyu.com/en/post/2018-03-11/understanding-the-bias-variance-decomposition-with-a-simulated-experiment/

https://www.r-bloggers.com/2019/06/simulating-the-bias-variance-tradeoff-in-r/

use this to create education-income simulation
https://daviddalpiaz.github.io/r4sl/simulating-the-biasvariance-tradeoff.html





