# Lecture 1

### 1. Explain how reinforcement learning differs from supervised and unsupervised learning in terms of the type of input the learning algorithms use to improve model performance. [5]

### 2. Explain why we need separate training and test data. What is generalization, and how does the concept relate to underfitting and overfitting? [10]

### 3. Define the three key components of Mitchell's definition of machine learning (Task $T$, Performance measure $P$, and Experience $E$). Give a concrete example for each component in the context of email spam classification. [10]

### 4. Explain the difference between classification and regression tasks. For each task type, provide: (a) the mathematical representation of the target variable, (b) a real-world example, and (c) one appropriate evaluation metric. [10]

### 5. Define the prediction function of a linear regression model and write down $L^2$-regularized mean squared error loss. [10]

### 6. Starting from the unregularized sum of squares error of a linear regression model, show how the explicit solution can be obtained, assuming $X^TX$ is invertible. [10]

# Lecture 2

### 1. Describe standard gradient descent and compare it to stochastic (i.e., online) gradient descent and minibatch stochastic gradient descent. Explain what it is used for in machine learning. [10]

### 2. Explain the relationship between model capacity and overfitting/underfitting. How does increasing polynomial degree in linear regression affect model capacity, and what are the consequences? [10]

### 3. Explain possible intuitions behind $L^2$ regularization. [5]

### 4. Explain the difference between hyperparameters and parameters. [5]

### 5. Write an $L^2$-regularized minibatch SGD algorithm for training a linear regression model, including the explicit formulas (i.e, formulas you would need to code it with numpy) of the loss function and its gradient. [10]

### 6. Does the SGD algorithm for linear regression always find the best solution on the training data? If yes, explain under what conditions it happens; if not, explain why it is not guaranteed to converge. What properties of the error function does this depend on? [10]

### 7. After training a model with SGD, you ended up with a low training error and a high test error. Using the learning curves, explain what might have happened and what steps you might take to prevent this from happening. [10]

### 8. You were given a fixed training set and a fixed test set, and you are supposed to report model performance on that test set. You need to decide what hyperparameters to use. How will you proceed and why? [10]

### 9. What methods can be used to normalize feature values? Explain why it is useful. [5]

### 10. You have a dataset with a categorical feature “color” with values {“red”, “green”, “blue”}. Explain why using integer encoding (red=0, green=1, blue=2) is problematic for linear regression. How would encode such feature instead? [10]
