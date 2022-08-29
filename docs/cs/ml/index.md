--- 
comment: True
---

# Machine Learning
!!!info
    The note here would tend to be quite theoretical. Some of the examples and sentences here are directly adopted from ISML, ESL, and COMP4432 Statistical Machine Learning lecture slides.

**Statistical learning** refers to learn from data, can be classified as supervised, semi-supervised and unsupervised:

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
<math xmlns="http://www.w3.org/1998/Math/MathML" display="block"><mrow data-mjx-texclass="ORD"><mtext mathvariant="bold">X</mtext></mrow><mo>=</mo><mrow data-mjx-texclass="INNER"><mo data-mjx-texclass="OPEN">[</mo><mtable columnspacing="1em" rowspacing="4pt"><mtr><mtd><msub><mi>x</mi><mrow><mn>11</mn></mrow></msub></mtd><mtd><msub><mi>x</mi><mrow><mn>12</mn></mrow></msub></mtd><mtd><mo>⋯</mo></mtd><mtd><msub><mi>x</mi><mrow><mn>1</mn><mi>p</mi></mrow></msub></mtd></mtr><mtr><mtd><msub><mi>x</mi><mrow><mn>21</mn></mrow></msub></mtd><mtd><msub><mi>x</mi><mrow><mn>22</mn></mrow></msub></mtd><mtd><mo>⋯</mo></mtd><mtd><msub><mi>x</mi><mrow><mn>2</mn><mi>p</mi></mrow></msub></mtd></mtr><mtr><mtd><mrow><mo>⋮</mo></mrow></mtd><mtd><mrow><mo>⋮</mo></mrow></mtd><mtd><mo>⋱</mo></mtd><mtd><mrow><mo>⋮</mo></mrow></mtd></mtr><mtr><mtd><msub><mi>x</mi><mrow><mi>n</mi><mn>1</mn></mrow></msub></mtd><mtd><msub><mi>x</mi><mrow><mi>n</mi><mn>2</mn></mrow></msub></mtd><mtd><mo>⋯</mo></mtd><mtd><msub><mi>x</mi><mrow><mi>n</mi><mi>p</mi></mrow></msub></mtd></mtr></mtable><mo data-mjx-texclass="CLOSE">]</mo></mrow></math>
<math xmlns="http://www.w3.org/1998/Math/MathML" display="block"><mi>y</mi><mo>=</mo><mrow data-mjx-texclass="INNER"><mo data-mjx-texclass="OPEN">(</mo><mtable columnspacing="1em" rowspacing="4pt"><mtr><mtd><msub><mi>y</mi><mn>1</mn></msub></mtd></mtr><mtr><mtd><msub><mi>y</mi><mn>2</mn></msub></mtd></mtr><mtr><mtd><mrow><mo>⋮</mo></mrow></mtd></mtr><mtr><mtd><msub><mi>y</mi><mi>n</mi></msub></mtd></mtr></mtable><mo data-mjx-texclass="CLOSE">)</mo></mrow></math>
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
$\textbf{y}$ is the observed output while $\textbf{X}$ is the observed input, $f$ is the actual function that maps $\textbf{X}$ into the ideal output, there's an error between observed and ideal output, and the **expected value** of error is 0.

A set of inputs $\textbf{X}$ is always available but it is not always easy for us to obtain the corresponding $\textbf{y}$. We denote $\hat{\textbf{y}}$ as an estimate of $\textbf{y}$, and $\hat{f}(\textbf{X})$ an estimate of $f(\textbf{X})$.

Then,
<math xmlns="http://www.w3.org/1998/Math/MathML" display="block"><mtable displaystyle="true" columnalign="right left right left right left right left right left right left" columnspacing="0em 2em 0em 2em 0em 2em 0em 2em 0em 2em 0em" rowspacing="3pt"><mtr><mtd></mtd><mtd><mtext>&nbsp;</mtext><mrow><mi mathvariant="double-struck">E</mi></mrow><mo stretchy="false">(</mo><mrow data-mjx-texclass="ORD"><mtext mathvariant="bold">y</mtext></mrow><mo>−</mo><mrow><mover><mrow data-mjx-texclass="ORD"><mtext mathvariant="bold">y</mtext></mrow><mo stretchy="false">^</mo></mover></mrow><msup><mo stretchy="false">)</mo><mn>2</mn></msup></mtd></mtr><mtr><mtd><mo>=</mo></mtd><mtd><mtext>&nbsp;</mtext><mrow><mi mathvariant="double-struck">E</mi></mrow><mo stretchy="false">[</mo><mo stretchy="false">(</mo><mi>f</mi><mo stretchy="false">(</mo><mrow data-mjx-texclass="ORD"><mtext mathvariant="bold">X</mtext></mrow><mo stretchy="false">)</mo><mo>−</mo><mrow><mover><mi>f</mi><mo stretchy="false">^</mo></mover></mrow><mo stretchy="false">(</mo><mrow data-mjx-texclass="ORD"><mtext mathvariant="bold">X</mtext></mrow><mo stretchy="false">)</mo><mo stretchy="false">)</mo><mo>+</mo><mi>ε</mi><msup><mo stretchy="false">]</mo><mn>2</mn></msup></mtd></mtr><mtr><mtd><mo>=</mo></mtd><mtd><mtext>&nbsp;</mtext><mo stretchy="false">[</mo><mi>f</mi><mo stretchy="false">(</mo><mrow data-mjx-texclass="ORD"><mtext mathvariant="bold">X</mtext></mrow><mo stretchy="false">)</mo><mo>−</mo><mrow><mover><mi>f</mi><mo stretchy="false">^</mo></mover></mrow><mo stretchy="false">(</mo><mrow data-mjx-texclass="ORD"><mtext mathvariant="bold">X</mtext></mrow><mo stretchy="false">)</mo><msup><mo stretchy="false">]</mo><mn>2</mn></msup><mo>+</mo><mn>2</mn><mo>⋅</mo><mtext>&nbsp;</mtext><mo>.</mo><mo>.</mo><mo>.</mo><mtext>&nbsp;</mtext><mo>⋅</mo><mrow><mi mathvariant="double-struck">E</mi></mrow><mo stretchy="false">(</mo><mi>ε</mi><mo stretchy="false">)</mo><mo>+</mo><mrow><mi mathvariant="double-struck">E</mi></mrow><mo stretchy="false">(</mo><msup><mi>ε</mi><mn>2</mn></msup><mo stretchy="false">)</mo></mtd></mtr><mtr><mtd><mo>=</mo></mtd><mtd><mtext>&nbsp;</mtext><mo stretchy="false">[</mo><mi>f</mi><mo stretchy="false">(</mo><mrow data-mjx-texclass="ORD"><mtext mathvariant="bold">X</mtext></mrow><mo stretchy="false">)</mo><mo>−</mo><mrow><mover><mi>f</mi><mo stretchy="false">^</mo></mover></mrow><mo stretchy="false">(</mo><mrow data-mjx-texclass="ORD"><mtext mathvariant="bold">X</mtext></mrow><mo stretchy="false">)</mo><msup><mo stretchy="false">]</mo><mn>2</mn></msup><mo>+</mo><mn>0</mn><mo>+</mo><mrow><mi mathvariant="double-struck">E</mi></mrow><mo stretchy="false">(</mo><msup><mi>ε</mi><mn>2</mn></msup><mo stretchy="false">)</mo><mo>−</mo><mrow><mi mathvariant="double-struck">E</mi></mrow><mo stretchy="false">(</mo><mi>ε</mi><msup><mo stretchy="false">)</mo><mn>2</mn></msup></mtd></mtr><mtr><mtd><mo>=</mo></mtd><mtd><mtext>&nbsp;</mtext><mi>V</mi><mi>a</mi><mi>r</mi><mo stretchy="false">(</mo><mi>ε</mi><mo stretchy="false">)</mo></mtd></mtr></mtable></math>