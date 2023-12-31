--- 
comment: True
---

# Machine Learning
!!!info
    Some of the examples and sentences here are directly adopted from MATH4432 Statistical Machine Learning lecture slides. The note is not complete and may have frequent modifications, and the knowledge about regression analysis is ignored as this is not the focus of the course.

**Statistical learning (统计学习)** refers to learn from data, can be classified as supervised, semi-supervised and unsupervised:

- supervised (with labeled output): prediction, estimation
- unsupervised (without labeled output): clustering
- semi-supervised (large amount of unlabeled data and small amount of labeled data) 

For prediction there are two types as well, regression (回归) refers to continuous or quantitative output value, otherwise would be classification (分类). 

Linear regression is the simplest regression method by using linear equations to approximate a certain function.

## Mathematical Formulations
$n$: Number of distinct data points

$p$: Number of features / predictors / variables

For instance, regarding `Wage`, there are 300 sample points where they are 10 factors like `year`, `age`, `race`, ... Then we have $n = 300$, $p = 10$.

We can use matrix $\textbf{X}$ to denote the data, and vector $\textbf{y}$ to denote output:

\begin{aligned}
\mathbf{X}=\left[\begin{array}{cccc}
x_{11} & x_{12} & \cdots & x_{1 p} \\
x_{21} & x_{22} & \cdots & x_{2 p} \\
\vdots & \vdots & \ddots & \vdots \\
x_{n 1} & x_{n 2} & \cdots & x_{n p}
\end{array}\right] 
\ \ \ \mathbf{y}=\left(\begin{array}{c}
y_{1} \\
y_{2} \\
\vdots \\
y_{n}
\end{array}\right)
\end{aligned}

The rows of $\textbf{X}$ refer to each data point, we can denote it as $x_i$:
<math xmlns="http://www.w3.org/1998/Math/MathML" display="block"><msub><mi>x</mi><mi>i</mi></msub><mo>=</mo><mrow data-mjx-texclass="INNER"><mo data-mjx-texclass="OPEN">(</mo><mtable columnspacing="1em" rowspacing="4pt"><mtr><mtd><msub><mi>x</mi><mrow><mi>i</mi><mn>1</mn></mrow></msub></mtd></mtr><mtr><mtd><msub><mi>x</mi><mrow><mi>i</mi><mn>2</mn></mrow></msub></mtd></mtr><mtr><mtd><mrow><mo>⋮</mo></mrow></mtd></mtr><mtr><mtd><msub><mi>x</mi><mrow><mi>i</mi><mi>p</mi></mrow></msub></mtd></mtr></mtable><mo data-mjx-texclass="CLOSE">)</mo></mrow></math>

The columns of $\textbf{x}$ refer to the collection of data points in terms of a certain feature, we can denote it as $\textbf{x}_j$:
<math xmlns="http://www.w3.org/1998/Math/MathML" display="block"><msub><mrow data-mjx-texclass="ORD"><mtext mathvariant="bold">x</mtext></mrow><mi>j</mi></msub><mo>=</mo><mrow data-mjx-texclass="INNER"><mo data-mjx-texclass="OPEN">(</mo><mtable columnspacing="1em" rowspacing="4pt"><mtr><mtd><msub><mi>x</mi><mrow><mn>1</mn><mi>j</mi></mrow></msub></mtd></mtr><mtr><mtd><msub><mi>x</mi><mrow><mn>2</mn><mi>j</mi></mrow></msub></mtd></mtr><mtr><mtd><mrow><mo>⋮</mo></mrow></mtd></mtr><mtr><mtd><msub><mi>x</mi><mrow><mi>n</mi><mi>j</mi></mrow></msub></mtd></mtr></mtable><mo data-mjx-texclass="CLOSE">)</mo></mrow></math>
Therefore,
<math xmlns="http://www.w3.org/1998/Math/MathML" display="block"><mrow data-mjx-texclass="ORD"><mtext mathvariant="bold">X</mtext></mrow><mo>=</mo><mrow data-mjx-texclass="INNER"><mo data-mjx-texclass="OPEN">(</mo><msub><mrow data-mjx-texclass="ORD"><mtext mathvariant="bold">x</mtext></mrow><mn>1</mn></msub><mtext>&nbsp;</mtext><mtext>&nbsp;</mtext><msub><mrow data-mjx-texclass="ORD"><mtext mathvariant="bold">x</mtext></mrow><mn>2</mn></msub><mtext>&nbsp;</mtext><mtext>&nbsp;</mtext><mo>⋯</mo><mtext>&nbsp;</mtext><mtext>&nbsp;</mtext><msub><mrow data-mjx-texclass="ORD"><mtext mathvariant="bold">x</mtext></mrow><mi>p</mi></msub><mo data-mjx-texclass="CLOSE">)</mo></mrow><mo>=</mo><mrow data-mjx-texclass="INNER"><mo data-mjx-texclass="OPEN">(</mo><mtable columnspacing="1em" rowspacing="4pt"><mtr><mtd><msubsup><mi>x</mi><mn>1</mn><mrow><mi>T</mi></mrow></msubsup></mtd></mtr><mtr><mtd><msubsup><mi>x</mi><mn>2</mn><mrow><mi>T</mi></mrow></msubsup></mtd></mtr><mtr><mtd><mrow><mo>⋮</mo></mrow></mtd></mtr><mtr><mtd><msubsup><mi>x</mi><mi>n</mi><mrow><mi>T</mi></mrow></msubsup></mtd></mtr></mtable><mo data-mjx-texclass="CLOSE">)</mo></mrow></math>

We can give this formula:
$$
\textbf{y} = f(\textbf{X}) + \varepsilon
$$
$\textbf{y}$ is the observed output while $\textbf{X}$ is the observed input, $f$ is the ground truth function that maps $\textbf{X}$ into the ideal output, there's an error between observed and ideal output, and we have the assumption that $\mathbb{E}(\varepsilon)=0$.

A set of inputs $\textbf{X}$ is always available but it is not always easy for us to obtain the corresponding $\textbf{y}$. We denote $\hat{\textbf{y}}$ as an estimate of $\textbf{y}$, and $\hat{f}(\textbf{X})$ an estimate of $f(\textbf{X})$.
## Basics
### Reducible and irreducible error
Then with $\mathbb{E}(\varepsilon)=0$ and an additional assumption that both $\hat{f}$ and $X$ is fixed for a moment, we have:

\begin{align}
\mathbb{E}(Y-\hat{Y})^2 & = \mathbb{E}[f(X) +\varepsilon - \hat{f}(X)]^2\\
&= \mathbb{E}[f(X) - \hat{f}(X)]^2 + 2 \mathbb{E}((f(X)-\hat{f}(X))\varepsilon) + \mathbb{E}(\varepsilon^2)\\
&= \mathbb{E}[f(X) - \hat{f}(X)]^2 + \mathbb{E}(\varepsilon^2) - \mathbb{E}(\varepsilon)^2\\
&= \mathbb{E}[f(X) - \hat{f}(X)]^2 + \text{Var}(\varepsilon)
\end{align}

$\mathbb{E}[f(X) - \hat{f}(X)]^2$ is the error that we could reduce by selecting the most appropriate model, but $\text{Var}(\varepsilon)$ is the irreducible error.

### Model choosing
To minimize the reducible error, we need to pick a suitable model to estimate model, there are two types of model, namely parametric and non-parametric.

For the parametric method, we first make an assumption about the form of the ground truth function e.g. linear. After the model has been selected, we use the training data to **fit** the model (train the parameters).

A simple example: we assume $f(X) = \beta_0 + \beta_1 X_1 + \beta_2 X_2 + \cdots + \beta_p X_p$. The the model is trained to find the values of the parameters: $\beta_0, \beta_1, \beta_2, \cdots, \beta_p$ s.t. $Y \approx \beta_0 + \beta_1 X_1 + \beta_2 X_2 + \cdots + \beta_p X_p$. The most common approach is called ordinary least squares which we will introduce in next section.

Non-parametric methods do not make explicit assumptions about the function form of $f$, so it is more flexible than non-parametric methods. Instead, they seek an estimate of $f$ s.t. it gets as close to the data points as possible without being too rough or wiggly.

<img src="../../assets/cs/ml/param_fit.jpg" alt="drawing" width="500"/>

As the photo has shown, the top-left is the ground truth function, the top-right is the parametric liner model (which is **underfitting** in this case), the bottom-left is the fitted spline model, and the bottom-right is a rough spline model with zero errors on the training data (which is called **overfitting**).

The definition of underfitting (not flexible enough) is that it performs poor in both training and testing data, while the definition of overfitting (too flexible) is that it performs very well in training data but poor in the testing data. We will further illustrate this in the bias-varience tradeoff part.

<img src="../../assets/cs/ml/flexbility_overfitting.jpg" alt="drawing" width="500"/>

The red curve is testing error and grey curve is training error. As you can see, when flexibility increases, the grey curve gradually fits into the noise, the training error reaches minimum point when flexibility equals to 5.

!!!note
    Note that model flexibility is not always proportional to the number of parameters. This holds for linear regression though.

Although we have many available models, **there is no free lunch in statistics**, which means there doesn't exist the so-called best model for all cases. On a particular data set, one specific method may work best, but some other method may work better on a similar but different set. 

### Measuring the quality of fit
We typically use mean-squared-error (MSE) given by

$$\text{MSE} = \frac{1}{n}{\sum_{i=1}^{n}{(y_i - \hat{f}(x_i))^2}}$$

There are train and test MSEs and our main interest is to choose the method that **minimise test MSE**.

To avoid the curve being too flexible, we normally use the **smoothing spline method**, where we add a special regularisation term:

$$\frac{1}{n}{\sum_{i=1}^{n}{(y_i - \hat{f}(x_i))^2}} + \lambda \int \hat{f}''(t) \ dt$$

$\lambda$ is the tuning parameter, when it is zero, we could allow the intergal value to be very large, so the model can be very flexible i.e. $y_i = \hat{f}(x_i)$, when it tends to infinity, as we want to minimize MSE, the model would be very rigid i.e. $y_i = \hat{a}x_i + \hat{b}$. So we can adjust the flexibility of the model by tuning the parameter.

### Bias & Variance tradeoff
As it is impossible for us to obtain all data, we could only obtain some training data $D$, but we want the model to work for all possible scenarios, so we want to find $\hat{f}$ to minimise

$$\mathbb{E}_{D}[f(x_0) - \hat{f}(x_0; D) \ | \ X = x_0]^2$$

($\hat{f}(x;D)$ is the estimated function from the data set $D$)

!!!notice
    Actually we can take one additional expectation w.r.t. $x_0$ here but it would make the formula too complicated.

Then we can derive the formula:

\begin{align}
& \ [f(x_0) - \hat{f}(x_0; D) \ | \ X = x_0]^2\\
=& \ [f(x_0) - \mathbb{E}_{D}(\hat{f}(x_0;D)) + \mathbb{E}_{D}(\hat{f}(x_0;D)) - \hat{f}(x_0; D) \ | \ X = x_0]^2\\
=& \ [f(x_0) - \mathbb{E}_{D}(\hat{f}(x_0;D)) \ | \ X = x_0]^2 + [\ \mathbb{E}_{D}(\hat{f}(x_0;D)) - \hat{f}(x_0; D) \ | \ X = x_0]^2\\
&+ 2 \ [f(x_0)-\mathbb{E}_{D}[\hat{f}(x_0;D)] \ | \ X = x_0] \ [\ \mathbb{E}_{D}(\hat{f}(x_0;D)) - \hat{f}(x_0; D) \ | \ X = x_0]
\end{align}

The first term is a constant. Inside the second term $\hat{f}(x_0;D)$ is still a random variable. For the third term, $[f(x_0)-\mathbb{E}_{D}[\hat{f}(x_0;D)] \ | \ X = x_0]$ is a constant but $[\ \mathbb{E}_{D}(\hat{f}(x_0;D)) - \hat{f}(x_0; D) \ | \ X = x_0]$ is a random variable.

Then we take expectation w.r.t. $D$ on both sides and we got the third term equal to 0 since 

$$\mathbb{E}_{D} [\ \mathbb{E}_{D}(\hat{f}(x_0;D)) - \hat{f}(x_0; D) \ | \ X = x_0] = \mathbb{E}_{D}(\hat{f}(x_0;D)) - \mathbb{E}_{D}(\hat{f}(x_0;D)) = 0$$

And the final result is

\begin{align}
& \ \mathbb{E}_{D}[f(x_0) - \hat{f}(x_0; D) \ | \ X = x_0]^2\\
=& \ \underbrace{[f(x_0) - \mathbb{E}_{D}(\hat{f}(x_0;D))] \ | \ X = x_0]^2}_{\text{Bias}^2} + \underbrace{\mathbb{E}_{D}{[\ \mathbb{E}_{D}(\hat{f}(x_0;D)) - \hat{f}(x_0; D) \ | \ X = x_0]^2}}_{\text{Variance}}\\
\end{align}

This formula tells us that we need to balance both bias and variance instead of only focusing on minimizing bias.

**Bias** refers to the error that is introduced by approximating a real-life problem, which may ne extremely complicated, by a much simpler model. The formula is quite easy to understand, is the difference between the ground truth value and the predicted value.

**Variance** refers to the amount by which $\hat{f}$ would change if we estimated it using a different training data set. Ideally the estimate for $f$ should not vary too much between training sets. The formula is basically a slightly complex version of $\mathbb{E}[(X - \mathbb{E}(X))^2]$.

For simple model, it has low variance but large bias. Example: $Y = f(X) + \varepsilon$, we simply let $\hat{f}(X) = 0$, then variance is 0, but bias is very large $f(X)^2$. For very flexible model, it has high variance but low bias. Example: $Y = f(X) + \varepsilon = 0 + \varepsilon$, where the ground truth function is $0$, we let $\hat{f}(x_i) = y_i$, which means it fits into the noise. Then $\text{Var}(\hat{f}(x_i)) = \text{Var}(y_i) = \text{Var}(\varepsilon_i) = \sigma_i^2$, bias is $f(x_i) - \mathbb{E}(\hat{f}(x_i)) = f(x_i) - \mathbb{E}(y_i) = 0$.

## Regression
### Simple Linear Regression
<img src="../../assets/cs/ml/simple_lr.jpg" alt="drawing" width="450"/>

Let $(x_1,y_1)$, $(x_2, y_2)$, $\cdots$, $(x_n,y_n)$ represent $n$ observation pairs, and we use simple linear regression $\hat{y_i} = \hat{\beta_0} + \hat{\beta_1}x_i$ to fit the data.

The error is defined as $e_i = y_i - \hat{y_i}$, then the residual sum of squares (RSS) is 
$$
e_1^2 + e_2^2 + \cdots + e_n^2 = \sum_{i=1}^{n}{(y_i - \hat{\beta_0} - \hat{\beta_1} x_i)^2}
$$
To minimise the error, we simply let $\frac{\partial (\text{RSS})}{\partial \hat{\beta_0}} = \frac{\partial (\text{RSS})}{\partial \hat{\beta_1}} = 0$, then with some algebraic operations we obtain:
$$
\hat{\beta_1} = \frac{\sum_{i=1}^{n}{(x_i - \overline{x})(y_i - \overline{y})}}{\sum_{i=1}^{n}{(x_i - \overline{x})^2}} \text{, } \ \hat{\beta_0} = \overline{y} - \hat{\beta_1} \overline{x}
$$
Actually, when we are drawing the contour map of RSS w.r.t. $\beta_0, \beta_1$, we can obtain the following ellipsoid shape:

<img src="../../assets/cs/ml/rss_contour.jpg" alt="drawing" width="500"/>

This is actually because of the squared term $(y_i - \hat{\beta_0} - \hat{\beta_1} x_i)^2$, term like this such as $\beta_0^2 + \beta_1^2 = 1$ can be written in the quadratic form (二次型):
 
\begin{bmatrix}
 \beta_0
 \\ \beta_1
\end{bmatrix}^{T}
\begin{bmatrix}
  1&0 \\
  0&1
\end{bmatrix}
\begin{bmatrix}
 \beta_0
 \\ \beta_1
\end{bmatrix}

The quadratic form like $\beta^{T}A \beta = C$ with A being positive and definite (正定) has ellipsoid shape.

After we have computed the values of coefficients, we can use our statistical knowledge to assess the accuracy of estimates. We assume the true relationship between $X$ and $Y$ is the following:
$$
Y = f(X) + \varepsilon, f(X) = \beta_0 + \beta_1 X
$$
Then as the graph shows

<img src="../../assets/cs/ml/accuracy_lr.jpg" alt="drawing" width="500"/>

Red line is the true relationship, which is known as **population regression line**, other lines are least squares lines computed based on separate random set of observations. Each line is different but on average the least squares lines are quite close to the population regression line.

We can use the **residual standard error (RSE)** to provide an absolute measure of lack of fit of the linear model (number of freedoms equals to the total number of parameters minus the two we want to estimate):
$$
\text{RSE} = \sqrt{\frac{1}{\text{degree of freedom}} \text{RSS}} = \sqrt{\frac{1}{n-2} \sum_{i=1}^{n}{(y_i - \hat{y_i})^2}}
$$

**R^2 statistics** provides an alternative measure of fit.

$$
\text{TSS} = \sum{(y_i - \overline{y})^2},  \
\text{RSS} = \sum{(y_i - \hat{y_i})^2}
$$
And $R^2$ is given by 

$$
R^2 = \frac{\text{TSS} - \text{RSS}}{\text{TSS}} = 1 - \frac{\text{RSS}}{\text{TSS}}
$$

The value is always between 0 and 1, if it is closed to 0, it means the prediction has similar effect as mean value, so it doesn't fit the data well; when it is closed to 1, it means the RSS is very small, that means it fits the data well.

### Multiple Linear Regression
#### Derivation of formula
The equation becomes much more complicated than before:
$$
Y = \beta_0 + \beta_1 X_1 + \beta_2 X_2 + \cdots + \beta_p X_p + \varepsilon
$$

Then our goal is to find $\hat{\beta} = [\hat{\beta_0}, \hat{\beta_1}, \cdots, \hat{\beta_p}]$ be the least squares

$$
\hat{\beta} = \text{argmin}_{\beta = [\beta_0, \beta_1, \cdots, \beta_p]}{\sum_{i=1}^{n}{(y_i - \beta_0 - \beta_1 x_{i1} - \cdots - \beta_{p} x_{ip})^2}}
$$

We know

\begin{aligned}
\mathbf{X}=\left[\begin{array}{ccccc}
\ 1 & x_{11} & x_{12} & \cdots & x_{1 p} \\
\ 1 & x_{21} & x_{22} & \cdots & x_{2 p} \\
\ \vdots & \vdots & \vdots & \ddots & \vdots \\
\ 1 & x_{n 1} & x_{n 2} & \cdots & x_{n p}
\end{array}\right] 
\ \ \ \mathbf{y}=\left(\begin{array}{c}
y_{1} \\
y_{2} \\
\vdots \\
y_{n}
\end{array}\right)
\end{aligned}

where the first column is representing the constant part (would time $\beta_0$ later).

Then the sum we want to minimise can be simplified as 
$\sum_{i=1}^{n}{(y_i - x_{\text{row i}} \ \beta)^2} \Rightarrow ||\mathbf{y} - \mathbf{X} \beta||_2^2$

!!!note
    $x_{\text{row i}}$ is row vector, $\beta$ is column vector 

    $\mathbf{X}: n \times (1 + p)$, $\mathbf{y}: n \times 1$, $\beta: (1 + p) \times 1$

\begin{align}
&||\mathbf{y}-\mathbf{X}\beta||\\
=& \ (\mathbf{y}-\mathbf{X}\beta)^{T} (\mathbf{y}-\mathbf{X}\beta)\\
=& \ (\mathbf{y}^T - \beta^T \mathbf{X}^T) (\mathbf{y}-\mathbf{X}\beta)\\
=& \ \mathbf{y}^T \mathbf{y} - \mathbf{y}^T \mathbf{X} \beta \ - \beta^T \mathbf{X}^T \mathbf{y} + \beta^T \mathbf{X}^T \mathbf{X}\beta\\ 
\end{align}

By dimension of $\mathbf{X}$, $\mathbf{y}$ and $\beta$, we know all four of the above are scalar, and in which $\mathbf{y}^T \mathbf{X} \beta$ is the transpose of $\beta^T \mathbf{X}^T \mathbf{y}$, so they are supposed to have the same value:

$$
\hat{\beta} = \text{argmin}_{\beta}{\ (\mathbf{y}^T \mathbf{y} - 2\mathbf{y}^T \mathbf{X} \beta \ + \beta^T \mathbf{X}^T \mathbf{X}\beta)}
$$
We take partial derivative w.r.t. $\beta$:

$$
0 - 2 \mathbf{X}^T \mathbf{y} + 2 \mathbf{X}^T \mathbf{X} \beta = 0
$$

!!! Note
    $$\frac{\partial a^T X}{\partial X} = a$$

    $$\frac{\partial X^T A X}{\partial X} = (A + A^{T}) X$$

Then

$$
\mathbf{X}^T \mathbf{X} \beta = \mathbf{X}^T \mathbf{y}
$$

So we finally obtain

\begin{align}
\hat{\beta} & = (\textbf{X}^T \textbf{X})^{-1} \textbf{X}^{T} \textbf{y}\\
\hat{y} & = X \hat{\beta}
\end{align}

#### Derive the mean and variance of beta estimate
By $\mathbf{y} = \mathbf{X}\beta + \varepsilon$ and $\varepsilon$ is a random noise vector, we know $\mathbf{y}$ is a random vector, so $\hat \beta$ is a random vector, and what follows is that $\hat y$ is a random vector as well. With this, we can compute the mean and variance of $\hat \beta$.

\begin{align}
\mathbb{E}(\hat \beta) &= \mathbb{E} [(\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T \mathbf{y}]\\
&= \mathbb{E} [(\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T (\mathbf{X} \beta + \varepsilon)]\\
&= \mathbb{E} [\mathbf{X}^{-1} (\mathbf{X}^T)^{-1} \mathbf{X}^T \mathbf{X} \beta \ + (\mathbf{X}^T \mathbf{X}^{-1})\mathbf{X}^T \varepsilon]\\
&= \beta + (\mathbf{X}^T \mathbf{X}^{-1})\mathbf{X}^T\mathbb{E}(\varepsilon)\\
&= \beta
\end{align}

!!! Note
    $$\text{Var}(\mathbf{z}) = \mathbb{E}(\mathbf{z} \mathbf{z}^T) - \mathbb{E}(\mathbf{z}) \mathbb{E}^{T}(\mathbf{z})$$

\begin{align}
\text{Var}(\hat \beta) &= \text{Var}\ [(\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T\mathbf{y}]\\
&= \text{Var}\ [(\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T (\mathbf{X}\beta + \varepsilon)]\\
&= \text{Var}\ [\beta + (\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T \varepsilon]\\
&= \text{Var}\ [(\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T \varepsilon]\\
&= \mathbb{E}[(\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T \varepsilon \ \cdot \varepsilon^T \mathbf{X}(\mathbf{X}^T \mathbf{X})^{-1}] -\underbrace{\mathbb{E}[(\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T \varepsilon]}_{0} \cdot \mathbb{E}^{T}[(\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T \varepsilon]\\
&= (\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T \ \mathbb{E}[ \varepsilon \cdot \varepsilon^T] \ \mathbf{X}(\mathbf{X}^T \mathbf{X})^{-1}\\
&= (\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T \  \sigma_{\varepsilon}^{2} \ \mathbf{X}(\mathbf{X}^T \mathbf{X})^{-1}\\
&=  \sigma_{\varepsilon}^{2} (\mathbf{X}^T \mathbf{X})^{-1}
\end{align}

!!! Derivation
    By independence of each element inside the random vector $\varepsilon$, $\text{Var}(\varepsilon) = \sigma_{\varepsilon}^{2} \ I_{n \times n}$.

    $$
    \sigma_{\varepsilon}^{2} \ I_{n \times n} = \text{Var}(\varepsilon) = \mathbb{E}(\varepsilon \varepsilon^T) - \mathbb{E}(\varepsilon) \mathbb{E}^{T}(\varepsilon) = \mathbb{E}(\varepsilon \varepsilon^T)
    $$

    e.g.: $\varepsilon = \begin{pmatrix}
    \varepsilon_1\\
    \varepsilon_2 
    \end{pmatrix}$, $\text{cov}\ \varepsilon = \begin{pmatrix}
    \sigma_{\varepsilon}^{2} & 0\\
    0 & \sigma_{\varepsilon}^{2}
    \end{pmatrix}$

#### Geometrical Interpretation of least squares regression

<img src="../../assets/cs/ml/geometric_lr.jpg"  width="400"/>

Like the graph suggests, $\text{col} \ \mathbf{X}$ is the hyperplane spanned by column vectors of $\mathbf{X}$. $\mathbf{X} \hat\beta$ could be considered as the linear combination of column vectors. Then our goal is to find $\hat \beta$ to minimise the norm of $\mathbf{y} - \mathbf{X} \hat{\beta}$, which can only be achieved when it is perpendicular to the hyperplane, that is, $\mathbf{X}^T (\mathbf{y} - \mathbf{X} \hat{\beta}) = 0$.

Verification:

\begin{align}
&\mathbf{X}^T (\mathbf{y} - \mathbf{X} (\mathbf{X}^T \mathbf{X})^{-1}\mathbf{X}^T \mathbf{y})\\
=& \ \mathbf{X}^T (\mathbf{I} - \mathbf{X} (\mathbf{X}^T \mathbf{X})^{-1}\mathbf{X}^T) \mathbf{y}\\
=& \ (\mathbf{X}^T - \mathbf{X}^T \mathbf{X} (\mathbf{X}^T \mathbf{X})^{-1}\mathbf{X}^T) \mathbf{y}\\
=& \ 0
\end{align}

There are several important issues we need to consider for linear regression. 

The first issue is that whether there is a relationship between the response and predictors, in other word, is at least one of the predictors useful in predicting the response. We can test the null hypothesis $H_0: \beta_1 = \beta_2 = \cdots = \beta_p = 0$ versus the alternative $H_a$: at least one $\beta_j$ is non-zero. This can be performed by using the F-statistics. Sometimes we only to test a particular subset of coefficients though. In hypothesis testing, type I error (expected error) and type II error (false negative) matter as well.

The second issue is deciding on important variables, this process is called **variable selection**. Ideally we can do that by trying a lot of different models and evaluating their qualities using methods like Akaike information criterion (AIC), Bayesian information criterion (BIC), adjusted $R^2$ etc. But in reality we cannot really try and test so many models, there are three classical approaches for this task: forward selection, backward selection, mixed selection. Lasso is one of the approach appeared in later years.

The third issue is model fit.

The fourth issue is prediction.

## Classification
This is the special case where outputs are discrete. Logistic regression, linear discrimant analysis and KNN would be investigated.

// image here

Under such scenario, linear regression does not work ideally, so we need to introduce new models.

### Logistic Regression
We let

\begin{cases}
\mathbb{P}(y_i = 1 | x_i) = \frac{1}{1 + \exp(-x_i^T \beta)} \textbf{ (sample version)}\\
\mathbb{P}(Y = 1 | X) = \frac{1}{1 + \exp(-\beta^T X)} \textbf{ (population version)}
\end{cases}