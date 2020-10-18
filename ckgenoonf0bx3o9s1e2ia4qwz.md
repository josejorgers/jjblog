## Learn the true polymorphism

Polymorphism is the ability of an object to take on many forms. It is an everyday word in Object-Oriented Programming (OOP). We have classes that inherit from other classes. We can treat an instance of a child class as an instance of the parent class, but it will behave differently.

Common OOP languages have a very limited version of polymorphism. In this post, I'm going to talk about a wider version of polymorphism that is included in Lisp.

## A first example

Let's see first what can be done with the polymorphism we commonly use.

As a simple example, we can have a class called ```Shape``` with a method called ```Area```. We also can have other classes called ```Triangle```, ```Rectangle```, ```Circle```, etc. Those classes inherit from ```Shape```, and each one provides its own implementation of the ```Area``` method.

We can make a list that only contains shape instances and treats every element as an instance of the ```Shape``` class.  But when we do  ```element.Area()``` we get different implementations every time. No matter that we treat every element as a ```Shape``` instance, every one of them behaves as a ```Triangle```, a ```Circle```, or a ```Rectangle``` according to its class.

That's the kind of polymorphism you might use in your favorite OOP language. But what if I tell you that's a very limited kind of polymorphism?

## Another example

In the previous example, we had a method inside a class that got overridden by every child class. That was good because we wanted to have different implementations of the ```Area``` method for every shape.

Let's think about another example. Now we have a class ```Drum```, and also let's create the class ```Stick```. Where do you include the ```Play``` method? Inside the ```Drum``` class? Inside the ```Stick``` class?

If you choose to include it inside ```Drum```, then you can have different implementations of  ```Play``` for every different drum you create. But what happens when you have a different stick? You can't have polymorphism with respect to the ```Stick``` instance! Exactly the opposite happens if you decide to include the method inside the ```Stick``` class: you'll have polymorphism on the ```Stick``` instances but not on the drums.

Where's your polymorphism now?!

## Taking a deeper look

Let's think about this issue. What happens when you do ```classInstance.ClassMethod(<args>)```?

We can say we are invoking the ```ClassMethod``` inside the ```classInstance```. But let's change the previous code a little bit:

```pseudocode
    ClassMethod(classInstance, <args>)
```

Now we see things slightly different. We are invoking the ```ClassMethod``` passing the ```classInstance``` as the first argument.

Maybe this is familiar to you. For example, in Python, you can invoke class methods using both ways interchangeably. Both of them are equivalent.
 
Using the second way, we can achieve different ```ClassMethod``` behavior by changing the ```classInstance``` argument. That's equivalent to polymorphism we use every day. That's called _polymorphism in the first argument_.

Let's return to our "drum and stick" example. The solution would be having something like this:

```pseudocode
Play(drumInstance, stickInstance, <args>)
```

Now we can have different behavior for every Drum-Stick combination. Nice!

But that's not a natural solution in many programming languages. Usually, every method has the "special" first parameter reserved for the instance of the class where it is defined. Hence, we can't have many "special" parameters representing many different instances of different classes. Even more, many OOP languages don't allow you to define methods outside of a class. So yes, you probably have been using a very limited version of polymorphism during your coding life.

Let's see what Lisp has to say!

## The Lisp solution

Lisp is a very old programming language. It's so old that there was no OOP when it was created. But in the '80s a new update of the language came up with the Common Lisp Object System (CLOS), And then, Lisp became an OOP language.

>As a matter of fact, CLOS is implemented in Lisp itself. There are a lot of great things about Lisp. I will definitely write about more of those things shortly.

There are no class methods in Lisp. Methods don't belong to the class. You have classes with properties and methods that can receive instances of those classes as arguments. It's the solution we talked about! That's how Lisp provides us with _polymorphism on multiple arguments_.

Of course, you can solve all kinds of problems with the "regular" polymorphism. You also can solve all kinds of problems without OOP at all! You can think about solving the Drum-Stick problem with a design pattern or some abstraction. But polymorphism in multiple arguments is explicit and simple.

Lisp's CLOS has a lot of more interesting features. This was the first one that really impressed me. Also was the first one I exploited a lot for solving some problems. Talking about other features of CLOS requires some prior knowledge of the language, but I plan to write about topics that don't demand such knowledge. Like this one!

Let's summarize what we have been talking about.

## Conclusions

Polymorphism is a cool feature in OOP. It is the ability of classes to behave differently, in different scenarios. But the kind of polymorphism that is present in popular OOP languages is limited.

Yes, there are different types of polymorphisms. We commonly use _polymorphism on the first argument_, but we can have _polymorphism on multiple arguments_. To achieve that, we need to separate methods from classes.

Lisp is a multiparadigm (Object-Oriented included) programming language. In Lisp, methods are created outside classes, and we can have polymorphism on multiple arguments.

Using this more general polymorphism you can get rid of some abstractions and design patterns that maybe you have been using to solve problems similar to the Drum-Stick one.

There are a lot of other features about Lisp that are mind-blowing. It really changes the way you think about programming. I plan to write about some of those features with a focus on the change of paradigm instead of the language.

But I recommend you to take a look at Lisp, and to experience those features and different ways of thinking by yourself.

If you liked the post let me know. You can leave a comment with whatever you want to say. You can also follow me on Twitter for debates and thoughts about related topics.