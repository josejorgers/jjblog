## Predictive Analytics in plain English

The field of Predictive Analytics is everywhere these days. We can hear about it in several and different areas like medicine, businesses, industries, and so on. But what exactly Predictive Analytics is? How can some algorithms predict the future, and what is the process to effectively apply Predictive Analytics? This article has the answers to these questions and other insights about this topic.

In the next section, we’ll be talking about what Predictive Analytics is and what are the fields it comprises. Then, we show different Predictive Analytics approaches.  Finally, we describe the stages that usually are part of the Predictive Analytics pipeline.

So, what do we mean by Predictive Analytics?

## What exactly Predictive Analytics is?

Predictive Analytics is an application of many techniques from Statistics and Artificial Intelligence, especially Machine Learning. It uses current and past facts to predict what to expect from the future.

For example, large industries can use Predictive Analytics to predict what would be the demand for some products and then adjust the production. Insurance companies use Predictive Analytics to determine whether an accident can occur with high probability and then charge more money.

Those analyses are carried on by statistical methods and Machine Learning techniques like Data Mining, Hypothesis Tests, and Predictive Modeling. The boom of these techniques is also motivated by technological advances that allow us to make more computations in much less time.

So, the idea is to accurately predict the future based on current and past data. That is why data has become one of the most valuable assets these days. Big companies like Google and Facebook are big mostly because of all the data they own. There is nothing to do without data. Data is the fuel of the Predictive Analytics engine.

If we have data, there are many ways to go, depending on what we want to do. The next section is about those different ways.

## Types of Predictive Analytics

Usually, we refer to Predictive Analytics as a set of techniques to calculate the likelihood of a future event. That can suggest that Predictive Analytics is all about predictive models that calculate some probabilities and that kind of stuff. While that is often the way to go, other approaches can be considered part of the Predictive Analytics spectrum. Furthermore, for some difficult tasks, it is possible to combine two or more approaches.

### Predictive Models

As stated above, this is the most frequent approach. We have many samples of past data, that are called training samples. For example, if we are trying to predict whether a certain transaction could be a fraud, we should have samples of past transactions. Every sample might have a date, a cardholder, the amount of money, the credit card number, and whether that specific transaction was fraudulent or not.

With those samples, we can train our model and it could be able to predict if a future transaction will be a fraud by knowing the rest of the attributes (cardholder, credit card number, etc). That prediction can be done by calculating a probability. Calculating how likely is a certain transaction to be fraudulent.

We can use predictive models to make other kinds of predictions. For example, instead of calculating a probability, we can calculate earnings for a future month, based on previous months. Our samples would be now the previous months, every month with attributes like the amount of money invested in advertising, the cost of production, and the earnings. Then we can train a model to predict future earnings. This can help businesses to make important decisions.

Examples of predictive models are Linear and Logistic Regression, Support Vector Machines, and Neural Networks.

### Descriptive Models

Sometimes we just want to discover some patterns or relationships between the samples. For example, a streaming company or an online retailer wants to recommend similar things to similar people. Then, we might need to identify some categories for customers. There will be customers that like technology, others that can have a low budget to spend, etc.

We can also use descriptive models to help us to understand better the data. And it could be just a previous step after which we can apply predictive or decision modeling.

Examples of Descriptive Models are Clustering and Principal Component Analysis.

### Decision Models

The last approach we’ll be talking about is decision modeling. This one is used when we want to find the relation between all the elements that influence a decision.

For example, in medicine, we want to build methods that help a doctor to diagnose a disease. But having a black-box that receives the symptoms and prints yes or no, or just pneumonia or heart attack is not useful. We need some explanation about why the model got that result. This way, the doctor can determine whether to trust the algorithm and it can also help the doctor to see some other insights about the patient.

In this approach, we find relationships between the attributes of the samples and make decisions. Examples of decision models are Decision Trees, Random Forests, and Bayesian Networks.

After describing the different ways we can encounter Predictive Analytics, let’s see a common pipeline of a Predictive Analytics task.

## Predictive Analytics pipeline

Although there are many ways to approach Predictive Analytics depending on the task to solve, we can split the process into some stages that are present in almost all solutions.

### 1. Data gathering, cleaning, and processing
This is the first and more difficult stage. It usually takes about 75-80% of the total time spent.

Data is not always easily available. Sometimes you might need to scrap websites and to process thousands of web pages to obtain that precious data. Other methods to obtain data could be even more time-consuming. For example, posting a poll, or a survey and waiting for people to fill them.

Cleaning data is about getting rid of samples and attributes that aren’t useful. Maybe we have samples with some missing attributes, or with values that are not correct. This step is extremely important because data often contain noise that can lead to terrible predictions.

Having clean samples doesn’t mean we have the data we need. We must always process the data. Sometimes we need the numerical values to be in a range between 0 and 1, or the literal values to be numerical values, and so on.

In this processing step, we can do what is called feature engineering. Feature engineering is a group of tricks to make our data even more suitable for the problem. We can make transformations between some attributes and obtain others that are more descriptive or that highlight a pattern.

### 2. Hypothesis Testing and data visualization
After processing our data we can state some hypotheses about that data, and how the future can be predicted from it. For example, we can assume that there is a linear relationship between some attributes of the samples. We can also assume that the behavior of an attribute is independent of the behavior of another attribute.

Those assumptions can be tested. Sometimes we can test whether they hold or not. This formal proof can be done via Hypothesis Testing, which is a powerful method to formally prove the validity of an assumption.

By visualizing some features of the samples we can also determine whether our assumptions seem to hold or not. Data visualization doesn’t give us formal proof but it is extremely helpful in practice. We can visualize some patterns or determine that some assumptions don’t seem to hold. Hence, data visualization is useful for both, understanding the data, and asserting whether a hypothesis seems to be correct.

### 3. Modeling, training, and validating
Once we have tested our hypothesis about the data, we are ready to build a model to predict the future.

Our model can be any of the examples mentioned in the previous section or others that were not mentioned. The model can even be a combination of several approaches and models.

The training process varies depending on the model and the approach we are following. But after the process is completed we should end with a result that should be validated. That means that we need to verify if our model is really solving the problem we want to solve.

The validation is commonly carried on with another set of samples that are not used in the training process. This way we can determine if our model can solve the task or if it “cheated” and just perform well with the training data. The validation process can also vary a lot depending on the approach we decided to follow.

After training and validating, we can obtain two results. If our model seems to perform well, then we can proceed to the next stage. But if our model is not performing as we expected, then we need to think about the cause of that underperformance. Maybe our hypotheses are wrong and we didn’t make a correct test. Or it could be some problems with the data (including not enough data). In any case, we need to return to a previous step and repeat part or all the process.

### 4. Productizing
By productizing, we mean to make the solution available to be used. It could be an online service like the recommendation system of Amazon or Netflix. It could be a desktop application to help a company in the decision-making process. The idea is to make the solution available to the end-user.

Cloud services have become a suitable and useful environment to host Predictive Analytics products in recent years.

There is one last stage. After the release of our product, a lot of new data will appear. After some time, the world will change significantly, and our model might get outdated. So, collecting new fresh data and retraining our model is the last step of this process that we can view now as a cycle.

## Conclusion

In this article, we explained what Predictive Analytics is, and how it is used.

Every single successful company these days uses Predictive Analytics. Actually, they are successful in part because of using Predictive Analytics.

This is a weapon that combines the power of Computer Science and Mathematics to predict the future and help to make that future better or worse for many people. By understanding the methods and processes involved in Predictive Analytics, we can help to make that future better for everyone.