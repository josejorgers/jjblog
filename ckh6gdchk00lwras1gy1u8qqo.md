## How Naive Bayes Classifiers Work – with Python Code Examples

Naive Bayes Classifiers (NBC) are simple yet powerful Machine Learning algorithms. They are based on conditional probability and Bayes's Theorem.

In this post, I explain "the trick" behind NBC and I'll give you an example that we can use to solve a classification problem.

In the next sections, I'll be talking about the math behind NBC. Feel free to skip those sections and go to the implementation part if you are not interested in the math.

In the implementation section, I'll show you a simple NBC algorithm. Then we'll use it to solve a classification problem. The task will be to determine whether a certain passenger on the Titanic survived the accident or not.

## Conditional probability

Before talking about the algorithm itself, let's talk about the simple math behind it. We need to understand what conditional probability is and how can we use Bayes's Theorem to calculate it.

Think about a fair die with six sides. What's the probability of getting a six when rolling the die? That's easy, it's 1/6. We have six possible and equally likely outcomes but we are interested in just one of them. So, 1/6 it is.

But what happens if I tell you that I have rolled the die already and the outcome is an even number? What's the probability that we have got a six now?

This time, the possible outcomes are just three because there are only three even numbers on the die. We are still interested in just one of those outcomes, so now the probability is greater: 1/3. What's the difference between both cases?

In the first case, we had no prior information about the outcome. Thus, we needed to consider every single possible result.

In the second case, we were told that the outcome was an even number, so we could reduce the space of possible outcomes to just the three even numbers that appear in a regular six-sided die.

In general, when calculating the probability of an event A, given the occurrence of another event B, we say we are calculating the conditional probability of A given B, or just the probability of A given B. We denote it by ```P(A|B)```.

For example, the probability of getting a six given that the number we have got is even: ```P(Six|Even) = 1/3```. Here we, denoted with Six the event of getting a six and with Even the event of getting an even number.

But, how do we calculate conditional probabilities? Is there a formula?

## How to calculate conditional probs and Bayes's Theorem

Now, I'll give you a couple of formulas to calculate conditional probs. I promise they won't be hard, and they are important if you want to understand the insights of the Machine Learning algorithms we'll be talking about later.

The probability of an event A given the occurrence of another event B can be calculated as follows:

```
P(A|B) = P(A,B)/P(B)
```

Where ```P(A, B)``` denotes the probability of both A and B occurring at the same time, and P(B) denotes the probability of B.

Notice that we need P(B) > 0 because it makes no sense to talk about the probability of A given B if the occurrence of B is not possible.

We can also calculate the probability of an event A, given the occurrence of multiple events B1, B2,..., Bn:

```
P(A|B1,B2,...,Bn) = P(A,B1,B2,...,Bn)/P(B1,B2,...,Bn)
```

There's another way of calculating conditional probs. This way is the so-called Bayes's Theorem.

```
P(A|B) = P(B|A)P(A)/P(B)

P(A|B1,B2,...,Bn) = P(B1,B2,...,Bn|A)P(A)/P(B1,B2,...,Bn)
```

Notice that we are calculating the probability of event A given event B, by inverting the order of occurrence of the events.

Now we suppose event A has occurred and we want to calculate the prob of event B (or events B1, B2,..., Bn in the second and more general example).

An important fact that can be derived from this Theorem is the formula to calculate ```P(B1, B2,..., Bn, A)```. That's called the chain rule for probabilities.

```
P(B1,B2,...,Bn,A) = P(B1 | B2, B3, ..., Bn, A)P(B2,B3,...,Bn,A)
= P(B1 | B2, B3, ..., Bn, A)P(B2 | B3, B4, ..., Bn, A)P(B3, B4, ..., Bn, A)
= P(B1 | B2, B3, ..., Bn, A)P(B2 | B3, B4, ..., Bn, A)...P(Bn | A)P(A)
```
That's an ugly formula, isn't it? But under some conditions, we can make a workaround and avoid it.

Let's talk about the last concept we need to know to understand the algorithms.

## Independence

The last concept we are going to talk about is independence. We say events A and B are independent if

```
P(A|B) = P(A)
```

That means that the prob of event A is not affected by the occurrence of event B. A direct consequence is that ```P(A,B) = P(A)P(B)```.

In plain English, this means that the prob of the occurrence of both A and B at the same time is equal to the product of the probs of events A and B occurring separately.

If A and B are independent, it also holds that:

```
P(A,B|C) = P(A|C)P(B|C)
```

Now we are ready to talk about Naive Bayes Classifiers!

## Naive Bayes Classifiers

Suppose we have a vector X of n features and we want to determine the class of that vector from a set of k classes y1, y2,..., yk. For example, if we want to determine whether it'll rain today or not.

We have two possible classes (k = 2): rain, not rain, and the length of the vector of features might be 3 (n = 3).

The first feature might be whether it is cloudy or sunny, the second feature could be whether humidity is high or low, and the third feature would be whether the temperature is high, medium, or low.

So, these could be possible feature vectors.

```
<Cloudy, H_High, T_Low>
<Sunny, H_Low, T_Medium>
<Cloudy, H_Low, T_High>
```

Our task is to determine whether it'll rain or not, given the weather features.

After learning about conditional probabilities, it seems natural to approach the problem by trying to calculate the prob of raining given the features:

```
R = P(Rain | Cloudy, H_High, T_Low)
NR = P(NotRain | Cloudy, H_High, T_Low)
```

If R > NR we answer that it'll rain, otherwise we say it won't.

In general, if we have k classes y1, y2, ..., yk, and a vector of n features ```X = <X1, X2, ..., Xn>```, we want to find the class yi that maximizes

```
P(yi | X1, X2, ..., Xn) = P(X1, X2,..., Xn, yi)/P(X1, X2, ..., Xn)
```

Notice that the denominator is constant and it does not depend on the class yi. So, we can ignore it and just focus on the numerator.

In a previous section, we saw how to calculate ```P(X1, X2,..., Xn, yi)``` by decomposing it in a product of conditional probabilities (the ugly formula):

```
P(X1, X2,..., Xn, yi) = P(X1 | X2,..., Xn, yi)P(X2 | X3,..., Xn, yi)...P(Xn | yi)P(yi)
```

Assuming all the features Xi are independent and using Bayes's Theorem, we can calculate the conditional probability as follows:

```
P(yi | X1, X2,..., Xn) = P(X1, X2,..., Xn | yi)P(yi)/P(X1, X2, ..., Xn)
= P(X1 | yi)P(X2 | yi)...P(Xn | yi)P(yi)/P(X1, X2, ..., Xn)
```

And we just need to focus on the numerator.

By finding the class yi that maximizes the previous expression, we are classifying the input vector. But, how can we get all those probabilities?

## How to calculate the probabilities

When solving these kind of problems we need to have a set of previously classified examples.

For instance, in the problem of guessing whether it'll rain or not, we need to have several examples of feature vectors and their classifications that they would be obtained from past weather forecasts.

So, we would have something like this:

```
...
<Cloudy, H_High, T_Low> -> Rain
<Sunny, H_Low, T_Medium> -> Not Rain
<Cloudy, H_Low, T_High> -> Not Rain
...
```

Suppose we need to classify a new vector <Coudy, H_Low, T_Low>. We need to calculate:

```
P(Rain | Cloudy, H_Low, T_Low) = P(Cloudy | H_Low, T_Low, Rain)P(H_Low | T_Low, Rain)P(T_Low | Rain)P(Rain)/P(Cloudy, H_Low, T_Low)
```

We get the previous expression by applying the definition of conditional probability and the chain rule. Remember we only need to focus on the numerator so we can drop the denominator.

We also need to calculate the prob for NotRain, but we can do this in a similar way.

We can find ```P(Rain) = # Rain/Total```. That means counting the entries in the dataset that are classified with Rain and dividing that number by the size of the dataset.

To calculate ```P(Cloudy | H_Low, T_Low, Rain)``` we need to count all the entries that have the features H_Low, T_Low, and Cloudy. Those entries also need to be classified as Rain. Then, that number is divided by the total amount of data. We calculate the rest of the factors of the formula in a similar fashion.

Making those computations for every possible class is very expensive and slow. So we need to make assumptions about the problem that simplify the calculations.

Naive Bayes Classifiers assume that all the features are independent from each other. So we can rewrite our formula applying Bayes's Theorem and assuming independence between every pair of features:

```
P(Rain | Cloudy, H_Low, T_Low) = P(Cloudy | Rain)P(H_Low | Rain)P(T_Low | Rain)P(Rain)/P(Cloudy, H_Low, T_Low)
```
Now we calculate ```P(Cloudy | Rain)``` counting the number of entries that are classified as Rain and were Cloudy.

>The algorithm is called Naive because of this independence assumption. There are dependencies between the features most of the time. We can't say that in real life there isn't a dependency between the humidity and the temperature, for example. Naive Bayes Classifiers are also called Independence Bayes, or Simple Bayes.

The general formula would be:

```
P(yi | X1, X2, ..., Xn) = P(X1 | yi)P(X2 | yi)...P(Xn | yi)P(yi)/P(X1, X2, ..., Xn)
```

Remember you can get rid of the denominator. We only calculate the numerator and answer the class that maximizes it.

Now, let's implement our NBC, and let's use it in a problem.

## Let's code!

I will show you an implementation of a simple NBC and then we'll see it in practice.

The problem we are going to solve is determining whether a passenger on the Titanic survived or not, given some features like their gender and their age.

Here you can see the implementation of a very simple NBC:

```python
class NaiveBayesClassifier:
    
    def __init__(self, X, y):
        
        '''
        X and y denotes the features and the target labels respectively
        '''
        self.X, self.y = X, y 
        
        self.N = len(self.X) # Length of the training set

        self.dim = len(self.X[0]) # Dimension of the vector of features

        self.attrs = [[] for _ in range(self.dim)] # Here we'll store the columns of the training set

        self.output_dom = {} # Output classes with the number of ocurrences in the training set. In this case we have only 2 classes

        self.data = [] # To store every row [Xi, yi]
        
        
        for i in range(len(self.X)):
            for j in range(self.dim):
                # if we have never seen this value for this attr before, 
                # then we add it to the attrs array in the corresponding position
                if not self.X[i][j] in self.attrs[j]:
                    self.attrs[j].append(self.X[i][j])
                    
            # if we have never seen this output class before,
            # then we add it to the output_dom and count one occurrence for now
            if not self.y[i] in self.output_dom.keys():
                self.output_dom[self.y[i]] = 1
            # otherwise, we increment the occurrence of this output in the training set by 1
            else:
                self.output_dom[self.y[i]] += 1
            # store the row
            self.data.append([self.X[i], self.y[i]])
            
            

    def classify(self, entry):

        solve = None # Final result
        max_arg = -1 # partial maximum

        for y in self.output_dom.keys():

            prob = self.output_dom[y]/self.N # P(y)

            for i in range(self.dim):
                cases = [x for x in self.data if x[0][i] == entry[i] and x[1] == y] # all rows with Xi = xi
                n = len(cases)
                prob *= n/self.N # P *= P(Xi = xi)
                
            # if we have a greater prob for this output than the partial maximum...
            if prob > max_arg:
                max_arg = prob
                solve = y

        return solve
```

Here, we assume every feature has a discrete domain. That means they take a value from a finite set of possible values.

The same happens with classes. Notice that we store some data in the __init__ method so we don't need to repeat some operations. The classification of a new entry is carried on in the classify method.

>This is a simple example of an implementation. In real-world applications, you don't need (and is better if you don't make) your own implementation. For example, the sklearn library in Python contains several good implementations of NBC's.

Notice how easy it is to implement it!

Now, let's apply our new classifier to solve a problem. We have a dataset with a description of 887 passengers on the Titanic. We also can see whether a given passenger survived the tragedy or not.

So our task is to determine if another passenger that is not included in the training set made it or not.

In this example, I'll be using the __pandas__ library to read and process the data. I don't use any other tool.

The data is stored in a file called titanic.csv, so the first step is to read the data and get an overview of it.

```python
import pandas as pd

data = pd.read_csv('titanic.csv')

print(data.head())
```

The output is:

```
Survived  Pclass                                               Name  \
0         0       3                             Mr. Owen Harris Braund   
1         1       1  Mrs. John Bradley (Florence Briggs Thayer) Cum...   
2         1       3                              Miss. Laina Heikkinen   
3         1       1        Mrs. Jacques Heath (Lily May Peel) Futrelle   
4         0       3                            Mr. William Henry Allen   

      Sex   Age  Siblings/Spouses Aboard  Parents/Children Aboard     Fare  
0    male  22.0                        1                        0   7.2500  
1  female  38.0                        1                        0  71.2833  
2  female  26.0                        0                        0   7.9250  
3  female  35.0                        1                        0  53.1000  
4    male  35.0                        0                        0   8.0500 
```

Notice we have the Name of each passenger. We won't use that feature for our classifier because it is not significant for our problem. We'll also get rid of the Fare feature because it is continuous and our features need to be discrete.

>There are Naive Bayes Classifiers that support continuous features. For example, the Gaussian Naive Bayes Classifier.

```python
y = list(map(lambda v: 'yes' if v == 1 else 'no', data['Survived'].values)) # target values as string

# We won't use the 'Name' nor the 'Fare' field

X = data[['Pclass', 'Sex', 'Age', 'Siblings/Spouses Aboard', 'Parents/Children Aboard']].values # features values
```

Then, we need to separate our data set into a training set and a validation set. The latter is used to validate how well our algorithm is doing.

```python
print(len(y)) # >> 887

# We'll take 600 examples to train and the rest to the validation process
y_train = y[:600]
y_val = y[600:]

X_train = X[:600]
X_val = X[600:]
```

We create our NBC with the training set and then classify every entry in the validation set.

We measure the accuracy of our algorithm by dividing the number of entries it correctly classified by the total number of entries in the validation set.

```python
## Creating the Naive Bayes Classifier instance with the training data

nbc = NaiveBayesClassifier(X_train, y_train)


total_cases = len(y_val) # size of validation set

# Well classified examples and bad classified examples
good = 0
bad = 0

for i in range(total_cases):
    predict = nbc.classify(X_val[i])
#     print(y_val[i] + ' --------------- ' + predict)
    if y_val[i] == predict:
        good += 1
    else:
        bad += 1

print('TOTAL EXAMPLES:', total_cases)
print('RIGHT:', good)
print('WRONG:', bad)
print('ACCURACY:', good/total_cases)
```

The output:

```
TOTAL EXAMPLES: 287
RIGHT: 200
WRONG: 87
ACCURACY: 0.6968641114982579
```

It's not great but it's something. We can get about a 10% accuracy improvement if we get rid of other features like Siblings/Spouses Aboard and Parents/Children Aboard.

You can see a notebook with the code and the dataset here

## Conclusions

Today, we have neural networks and other complex and expensive ML algorithms all over the place.

NBCs are very simple algorithms that let us achieve good results in some classification problems without needing a lot of resources. They also scale very well, which means we can add a lot more features and the algorithm will still be fast and reliable.

Even in a case where NBCs were not a good fit for the problem we were trying to solve, they might be very useful as a baseline.

We could first try to solve the problem using an NBC with a few lines of code and little effort. Then we could try to achieve better results with more complex and expensive algorithms.

This process can save us a lot of time and gives us immediate feedback about whether complex algorithms are really worth it for our task.

In this article, you read about conditional probabilities, independence, and Bayes's Theorem. Those are the Mathematical concepts behind Naive Bayes Classifiers.

After that, we saw a simple implementation of an NBC and solved the problem of determining whether a passenger on the Titanic survived the accident.

I hope you found this article useful. You can read about Computer Science related topics in my personal blog and by following me on [Twitter](https://twitter.com/josejorgexl).