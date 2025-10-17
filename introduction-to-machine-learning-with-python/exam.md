## Lecture 1

#### 0. Not asked, but might be useful

- Finding weight values means minimizing an error function between the real target values and their predictions:
  - popular and simple error function is MSE (mean squared error):
    - $MSE(w) = \frac{1}{N} \sum_{i=1}^N (y(x_i,w) - t_i)^2$
    - where we have a dataset of $N$ input values $x_1,...,x_N$ and targets $t_1,...,t_N$
    - often the $\frac{1}{N}$ is replaced by $\frac{1}{2}$, which for minimizing is equal, but the math comes out nicer

#### 1. Explain how reinforcement learning differs from supervised and unsupervised learning in terms of the type of input the learning algorithms use to improve model performance. [5]

- **Supervised learning**
  - as a dataset, we are given inputs and their respective outputs (labels or targets).
  - the goal is to find a model that, given an input, correctly assigns the correct output.
- **Unsupervised learning**
  - as a dataset, only inputs without desired outputs are given (raw text, raw images).
  - the goal is typically to divide the inputs into groups of similar inputs (clustering), or to learn how inputs look and generate new data (generative models).
- **Reinforcement leaning**
  - the goal is to learn the behavior (policy) of an agent so that it best solves a given problem based on feedback (in form of rewards or punishments) from the environment in which it acts.
  - We can think of this as trying to learn a new game by trying different actions and seeing how they affect the score.

#### 2. Explain why we need separate training and test data. What is generalization, and how does the concept relate to underfitting and overfitting? [10]

- **Optimization vs Generalization**
  - **Optimization** tries to match the training data as well as possible
  - **Generalization** tries to match previously unseen data as well as possible, which is ML's goal

- **Why separate train and test data?**
  - if we wouldn't separate train and test data, the model could "cheat" and exploit the data and perform well for them, however, ML's goal is to perform best as possible for all data, including **unseen** data, which in this case wouldn't be followed at all
  - that is why we separate the dataset into train and test data, where we train the model on the train data and then test it on the test data, which act as mentioned **unseen** data
  - note: we also keep a validation data for hyperparamaters

- **How underfitting/overfitting relates to generalization?**
  - these two terms represent the result of poor generalization
  - **Underfitting**:
    - model is too simple and small, causing both train and test errors high (shown by low-degree polynomial case)
  - **Overfitting**:
    - model fits in the train set extremely well, causing train error too low, however, this doesn't follow the ML's goal, generalization, because the test error is high

#### 3. Define the three key components of Mitchell's definition of machine learning (Task $T$, Performance measure $P$, and Experience $E$). Give a concrete example for each component in the context of email spam classification. [10]

A computer program is said to learn from experience $E$ with respect to some class of tasks $T$ and performance measure $P$, if its performance at tasks in $T$, as measured by $P$, improves with experience $E$.

- Task $T$
  - represents type of problem being solved:
    - **classification**: assign one of $k$ categories to a given input $x$
    - **regression**: predict a real target $t \in R$ for a given input $x$
    - **structured prediction**: predict a structured object (sequence/tree/graph), e.g. a label for each token in an email or a translation of the email
    - **denoising**: given a corrupted input, recover a clean version, e.g. remove HTML noise/boilerplate from emails
    - **density estimation**: learn $p(x), the probability distribution of inputs, used for anomaly detection (flag email that look like potential spam)
  
  - concrete example for email spam:
    - classification - decide if email are spam or ham (categories: ${spam, ham}$

- Performance measure $P$
  - represents how we measure success:
    - **accuracy (#correct/#all), error rate, F-score**
  - concrete example for email spam:
    - accuracy - we label 900 email correctly as spam or ham out of 1000 emails, getting accuracy $0.9$
    - F1-score - harmonic mean of **precision** and **recall** for the spam class, e.g. Let's say among $200$ **actual spam** and $800$ **ham** emails, our model (classifier) predicts $180$ **spam**, of which $150$ **are actual spam**, we get:
      - **precision**: $150/180 = 0.833$
      - **recall**: $150/200 = 0.75$
      - **F1** $= 2 * \frac{0.833*0.75}{0.833+0.75} = 0.789$

- Experience $E$
  - represents the data the learner uses to improve:
    - **supervised, unsupervised, reinforcement learning**
  - concrete example for email spam:
    - supervised learning - dataset contains emails with labels if they are spam or ham

#### 4. Explain the difference between classification and regression tasks. For each task type, provide: (a) the mathematical representation of the target variable, (b) a real-world example, and (c) one appropriate evaluation metric. [10]

- Let $x \in R^D$ be the input, two basic ML tasks are:

- Regression tasks:
  - **a) mathematical representation**:
    - **target**: $t \in R$
    - the goal is to predict **target** variable for a given $x$
  - **b) real-world example**
    - Predict apartment price, temperature tomorrow, energy consumption next hour
  - **c) appropriate evaluation metric**
    - MSE, RMSE

- Classification tasks:
  - **a) mathematical representation**:
    - **target**: $t \in {0, 1, ..., K-1}$ where ${0, 1, ..., K-1}$ is a fixed set representing labels/classes/categories 
    - the goal is to choose a corresponding label/class/category for given $x$
    - we can predict the class only or the whole distribution of all classes probabilities
  - **b) real-world example**
    - email spam vs ham
  - **c) appropriate evaluation metric**
    - Accuracy, F1-score

#### 5. Define the prediction function of a linear regression model and write down $L^2$-regularized mean squared error loss. [10]

- Prediction function of a linear regression model:
  - $y(x;w,b) = x_1 w_1 + x_2 w_2 + ... + x_D w_D + b = \sum_{i=1}^D {x_i w_i} + b = x^T w + b$
  - $y(x;w) = x^T w$, if we use bias trick to avoid dealing with it separately, we enlarge input vector $x$ by padding a value 1, the bias is encoded by the weights. 
  - $w$ are called weights and $b$ bias

- $L^2$-regularized mean squared error loss:
  - tries to prefer "simpler" models by endorsing models with smaller weights
  - penalizes models with large weights by utilizing the following error function:
    - $\frac{1}{2} \sum_{i=1}^N (y(x_i;w) - t_i)^2 + \frac{λ}{2} \lVert w \rVert^2$
   
    - Matrix form: $\frac{1}{2} \lVert Xw - t \rVert^2 + \lVert w \rVert^2$

#### 6. Starting from the unregularized sum of squares error of a linear regression model, show how the explicit solution can be obtained, assuming $X^TX$ is invertible. [10]

- Unregularized sum of squares error of a linear regression model:
  - our goal is to minimize this error function:
    - $\frac{1}{2} \sum_{i=1}^N (y(x_i;w) - t_i)^2$
  - if we denote $X \in R^{N x D}$ the matrix of input values with $x_i$ on a row $i$ and $t \in R^N$ the vector of target values, we can write it as:
    - $\frac{1}{2} \lVert Xw - t \rVert^2$, because:
      - $\frac{1}{2} \lVert Xw - t \rVert^2 = \sum_i ((Xw - t)_i)^2 = \sum_i ((Xw)_i - t_i))^2 = \sum_i (x_i^T w - t_i)^2$
  - then we can take partial derivates with respect to each component $w_j$ and set to zero
    - $\frac{∂}{∂ w_j} \frac{1}{2} \sum_i^N (x_i^T w - t_i)^2 = \frac{1}{2} \sum_i^N (2(x_i^T w - t_i) x_{ij}) = \sum_i^N x_{ij}(x_i^T w - t_i)$
    - so we want for all $j$ that $\sum_i^N x_{ij}(x_i^T w - t_i) = 0$
  - we can then rewrite the explicit sum into matrix form
    - let $X_{*,j}$ denote the $j-th$ column of $X$, we get
      - $X_{*,j}^T (Xw - t) = 0$ iff $X^T (Xw-t) = 0$
    - which we can rewrite to
      - $X^T Xw = X^T t$
  - the matrix $X^T X$ is of size $D x D$ and with the assumption that $X^T X$ is invertible, we cna compute its inverse and get
    - $w = (X^TX)^{-1} X^T t$

## Lecture 2

#### 1. Describe standard gradient descent and compare it to stochastic (i.e., online) gradient descent and minibatch stochastic gradient descent. Explain what it is used for in machine learning. [10]



#### 2. Explain the relationship between model capacity and overfitting/underfitting. How does increasing polynomial degree in linear regression affect model capacity, and what are the consequences? [10]

- What is model capacity?
  - 

- Relationship between model capacity and overfitting/underfitting
  - by modifying capacity, we can control whether a model underfits or overfits

- How does increasing polynomial degree in linear regression affect model capacity?
  -  with increasing polynomial degree $M$, linear regression fits more complex curves on the polynomial features $x = (x^0,...,x^M)$

- What are the consequences?
  - low model capacity causes underfitting
  - high model capacity causes overfitting

Representation capacity
Effective capacity

#### 3. Explain possible intuitions behind $L^2$ regularization. [5]

- If we have more models with some acceptable generalization error, then it makes sense to choose the most simple one, becuase
  - it probably has simpler rules and doesnt remember every single result
    - this means e.g that the changes inside the model will probably be smaller as well  

#### 4. Explain the difference between hyperparameters and parameters. [5]

**Parameters**
  - e.g. $w$ or $b$,
  - learned from the training data when training the model

**Hyperparameters**
  - are parameters that cannot be learned from the training data, but need to set by us, in order to just start traning
  - are not adapted by the learning algorithm itself
  - e.g. polynomial degree $M$, $L^2$ strength $λ$, learning rate $α$
  - How to set those hyperparameters?
    - we have a dataset on input, we create:
      - train data $~ 80%$ of data from the dataset
      - validation set $~ 10%$ of data from the dataset
      - test set $~ 10%$ of data from the dataset
    - when we want to test couple of hyperparameters, we want to train multiple times, but we need to evaluate it
      - for that, we create a validation set, on which we evaluate the trained model with tried hyperparameters
      - if we were to use the test set for the evaluation, we would, see and train on the test data, which are supposed to act as **unseen** data, so we would violate generalization

#### 5. Write an $L^2$-regularized minibatch SGD algorithm for training a linear regression model, including the explicit formulas (i.e, formulas you would need to code it with numpy) of the loss function and its gradient. [10]



#### 6. Does the SGD algorithm for linear regression always find the best solution on the training data? If yes, explain under what conditions it happens; if not, explain why it is not guaranteed to converge. What properties of the error function does this depend on? [10]



#### 7. After training a model with SGD, you ended up with a low training error and a high test error. Using the learning curves, explain what might have happened and what steps you might take to prevent this from happening. [10]

- What happened?
  - low training error and high test error sounds like a overfitting the model

- What steps to take to prevent this?
  - if its too late, we can:
    - regularize the data, or
    - take the best validation checkpoint (e.g. early stopping)
  - otherwise we can
    - increase $L^2$ strength $λ$
    - reduce capacity (lower polynomial degree $M$)
    - collect more data
    - early stop on validation loss

#### 8. You were given a fixed training set and a fixed test set, and you are supposed to report model performance on that test set. You need to decide what hyperparameters to use. How will you proceed and why? [10]

Already mentioned inside [here](#4-explain-the-difference-between-hyperparameters-and-parameters-5)

#### 9. What methods can be used to normalize feature values? Explain why it is useful. [5]

Common solutions:

- Normalization:
  - $x_{i,j}^{norm} = \frac{x_{i,j} - min_k x_{k,j}}{max_k x_{k,j} - min_k x_{k,j}}$
  - `MinMaxScaler` in Skicit-learn

- Standardization:
  - $x_{i,j}^{standard} = \frac{x_{i,j} - \hat{μ_j}}{\hat{σ_j}}$
  - `StandardScaler` in Skicit-learn

- Why is it useful?
  - features on different scales "pull" the gradient unequally, normalization lets one $α$ work and speeds/steadies convergence

#### 10. You have a dataset with a categorical feature “color” with values {“red”, “green”, “blue”}. Explain why using integer encoding (red=0, green=1, blue=2) is problematic for linear regression. How would encode such feature instead? [10]

- The main problem with integer encoding is that linear regression treats differences (green - red = 1) as meaningful distances/order, which is arbitrary for categories, creating fake ordering.

- I would encode such feature with One-Hot encoding. The encoding in colors case would look like this:
  - there are $3$ colors - red, green, blue
  - create a value for each of them with binary vector of length $3$ with $1$ at one position inside the vector.
  - $red = [1,0,0]$, $green = [0,1,0]$ and $blue = [0,0,1]$
  - this solves the problem, because the model learns a separate weight per category

## Lecture 3

#### 1. Define binary classification, write down the perceptron algorithm, and show how a prediction is made for a given data instance $x$. [10]

- Binary classification
  - 

- Perceptron algorithm
  - **Input**: Linearly separable dataset ($X \in R^{N x D}, t \in {-1, 1}^N$)
  - **Output**: Weights $w \in R^D$ such that $t_i w_i^T w > 0$ for all $i$
  - **Steps**:
    - $w <- 0$
    - until all examples are classified correctly, process example $i$:
      - $y <- x_i^T w$
      - if $t_i y <= 0$ (incorrectly classified example):
        - $w <- w + t_i x_i$

- Given data instance $x$
  - 

#### 2. Explain what it means for a dataset to be linearly separable. Give an example of a simple 2D dataset that is not linearly separable and explain why the perceptron algorithm would fail on it. [10]

- Set is called linearly separable, if there exists a weight vector $w$ such that this equation holds:
  - Assuming the target value $t \in {-1,1}$, the goal is to find weights $w$ such that for all train data:
    - $sign(y(w_i;w)) = sign(w_i^Tw) = t_i$, or equivalently
    - $t_i y(x_i;w) = t_i x_i^T w > 0$
   
- Example of 2D dataset not linearly separable and why would perceptron algorithm fail
  - the reason the perceptron algorithm would fail is that it would never finish, because the condition inside the algorithm is that all examples are classified correctly...which for not linearly separable set is not possible
  - example:
    - 

#### 3. For discrete random variables, define entropy, cross-entropy, and Kullback-Leibler divergence, and prove the Gibbs inequality (i.e., that KL divergence is non-negative). [20]

- Entropy
  - amount of surprise in the whole distribution
  - $H(P) = E_{x~P} [I(x)]= - E_{x~P} [log P(x)]$
    - for discrete $P$: $H(P) = - \sum_x P(x)log P(x)$ (for $P(x) = 0$ we consider $P(x)log P(x) to be zero$
    - for continuous $P$: $H(P) = - \int P(X)log P(x) dx$ (differential entropy - can be negative)

- Cross-entropy
  - $H(P,Q) = -E_{x~P}[log Q(x)]$
    - where:
      - bude merat rozdiel medzi dvoma distribuciami

- Gibbs Inequality and its proof
  - part 1: $H(P,Q) >= H(P)$
  - part 2: $H(P) = H(P,Q) \iff P = Q$
  - proof:
    - part 1:
      - consider $H(P) - H(P,Q) = \sum_x P(x) log \frac{Q(x)}{P(x)}$
      - using the fact that $log(x) <= (x-1)$ with equality only for $x = 1$, we get:
        - $\sum_x P(x) log \frac{Q(x)}{P(x)} <= \sum_x P(x) (\frac{Q(x)}{P(x)} - 1) = \sum_x Q(x) - \sum_x P(x) = 0
        - also the picture, displaying the fact mentioned above: <img width="620" height="442" alt="image" src="https://github.com/user-attachments/assets/0ed20979-f8b9-4830-b9da-1afb004699a6" />
    - part 2:
      - For the equality to hold, \frac{Q(x)}{P(x)} must be $1$ for all $x$, e.g. $P = Q$

- Kullback-Leibler (KL) divergence
  - also called **relative entropy**
  - $D_{KL}(P \lVert Q) = H(P, Q) - H(P) = E_{x~P}[log P(x) - log Q(x)]$

#### 4. Explain the notion of likelihood in machine learning. What likelihood are we estimating, and why do we do it? [10]

#### 5. Describe maximum likelihood estimation as minimizing NLL, cross-entropy, and KL divergence and explain whether they differ or are the same and why. [20]

#### 6. Provide an intuitive justification for why cross-entropy is a good optimization objective in machine learning. What distributions do we compare in cross-entropy? Why is it good when the cross-entropy is low? [5]

#### 7. Considering the binary logistic regression model, write down its parameters (including their size) and explain how we decide what classes the input data belong to (including the explicit formula for the sigmoid function). [10]

#### 8. Write down an $L^2$-regularized minibatch SGD algorithm for training a binary logistic regression model, including the explicit formulas (i.e., formulas you would need to code it in numpy) of the loss function and its gradient (saying just ∇ is not enough). [20]

#### 9. Compare and contrast perceptron and logistic regression by discussing: (a) what each algorithm optimizes, (b) whether each provides probability estimates, (c) whether each is guaranteed to converge, and (d) the quality of solutions each finds. [10]
