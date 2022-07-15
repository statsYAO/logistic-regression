# logistic-regression
```
library(titanic)
data("titanic_train")
Titanic_mod = lm(Survived ~ Fare, data = titanic_train)
summary(Titanic_mod)

plot(Survived ~ Fare, data = titanic_train)
abline(Titanic_mod, col = "red")   
```

abline() function in R Language is used to add one or more straight lines to a graph. The abline() function can be used to add vertical, horizontal or regression lines to plot. 

```
plot(jitter(Survived, amount = 0.1) ~ Fare, ylim = c(-0.25, 1.25), data = titanic_train)
abline(Titanic_mod, col = "red")
```

The jitter R function adds noise to a numeric vector.
ylim: set the limits of the y axis.

```
plot(Titanic_mod, c(1,2,5))
```

Diagnostic Plots for Linear Regression Analysis. The above code gives us in the order: Residuals vs Fitted, Normal Q-Q, Residuals vs Leverage.

```
Titanic_logitmod = glm(Survived ~ Fare, family = binomial, data = titanic_train)
plot(Survived ~ Fare, data = titanic_train)
B0 = summary(Titanic_logitmod)$coef[1]
B1 = summary(Titanic_logitmod)$coef[2]
curve(exp(B0+B1*x) / (1 + exp(B0+B1*x)), add = TRUE, col = "red")
```

curve: Draw Function Plots; if TRUE add to an already existing plot;

In the Binary Logistic Regression Model, $\pi$ = exp(B0+B1*x) / (1 + exp(B0+B1*x)) = proportion of 1's (yes, success, ...) at any x = Probability form

```
set.seed(05312022)
passanger = titanic_train[sample(nrow(titanic_train) , 1) ,]
predict(Titanic_logitmod, passanger, type = "response")
```

```
library(Stat2Data)
data("Putts1")
View(Putts1)
Putts.table =  table(Putts1$Made, Putts1$Length)
p.hat = as.vector(Putts.table[2,] / colSums(Putts.table))
p.hat
```

$\hat{p}$ = # made / # trials

```
logit = function(BO, B1, x)
   {
     exp(B0+B1*x) / (1 + exp(B0+B1*x))
     }
pi.hat = logit(B0, B1, c(3:7))

Putts = data.frame(
  "Length" = c(3:7),
  "p.hat" = p.hat,
  "pi.hat" = pi.hat)
```

# Odds
The odds against a certain horse winning a race are 4 to 1. What does that mean?

4 losses for every 1 win

P(Win) = $\frac{1}{5}$

P(Loss) = $\frac{4}{5}$

Odds = $\frac{P(Win)}{P(Loss)}$ = $\frac{1}{4}$

If $\pi$ = proportion of "yes" (success, 1, ....), the odds of yes is $\frac{P(yes)}{P(no)}$ = $\frac{\pi}{1-\pi}$

With a little bit of algebra, $\pi$ = $\frac{odds}{1+odds}$

# Odds and Logistic Regression

Logit form: log(odds) = log($\frac{\pi}{1-\pi}$) = $\beta_0$ + $\beta_1$ X

The logistic model assumes a linear relationship between the predictor and log(odds).

$\hat{odds}$ = # made / # missed = $\frac{\hat{p}}{1-\hat{p}}$ (from sample)

$\hat{odds}$ =  $\frac{\hat{\pi}}{1-\hat{\pi}}$ (from logistic regression)

```
Putts$p.odds = Putts$p.hat / (1 - Putts$p.hat)
plot(log(p.odds) ~ Length, data = Putts, xlim = c(2,8), ylim = c(-2,3))
abline(B0, B1, col = "red")
```

abline a, b: It specifies the intercept and the slope of the line
