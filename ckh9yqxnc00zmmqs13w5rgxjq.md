## A new language for every new problem

What if I tell you that you can always have the language that better fits your problem?

We are always implementing design patterns and abstractions to adapt our programming language to the domain of the problem. We create the classes ```Store```, ```Product```, and ```Customer```. We create functions to represent the operations of selling, buying, update the inventory, etc. But all those building blocks are always expressed in terms of a general-purpose language that was not thought to fit our very immediate necessities.

In this article, I'll talk about the paradigm that is used in the Lisp programming language. With Lisp, we not only create a program, but we also create a specific language at the same time. Thus, we end up with a Domain-Specific Language (DSL) and a short, elegant, and clean program.

## Understanding the point

We always create new entities and operators when programming. We implement functions with meaningful names that express part of the domain of our problem. But those functions and classes are created in the same way, with a rigid syntactical structure. No matter how elaborated your abstractions are, you always build them in the same way. For example, creating a class in Python will always be like:

```python
class <ClassName>(<ParentClass>):
    <ClassBody>
 ```

Maybe you can imagine another more expressive way to create classes in the domain you are working in, but there is no way to change that syntax into a kind of shortcut to create those domain-specific classes. You can solve a lot of problems with inheritance and other mechanisms,  but a function always needs to be built with the same syntactical structure, and the same happens with classes.

You can redefine operators like ```+```, and ```==```. But you can't modify what the ```class``` reserved word does. You can't add a new operator to create classes neither.

All the programs you create in Python are written with the same vocabulary. Your coding expertise can make the code to be expressive, but no matter the expertise, that expressiveness will always have a limit. A limit imposed by the syntax of the programming language and its reserved words.

Most of the developer community is so used to these limitations that not even question them. We learn to code in a specific language, we learn some algorithms and the way we can translate them to that programming language. We get used to build programs in that way.

But there's another way. I just want to make a little detour right now and then dive into the main topic.

## Limitations of C-like macros

If you have programmed in C/C++, Assembly, or some other language that supports macros (but not the Lisp-like macros), you could think that the previous section problem does not apply to that language.

There is a big difference between C-like macros and what we're going to talk about in the next section.

With C-like macros, we can define a pattern and then substitute any occurrence of that pattern in our program for a piece of code, in compilation time. For example, we can do

```c
#define MAX 1000000
...

int array[MAX];
...
```

Before actually compiling the program, the C compiler substitutes any occurrence of ```MAX``` for ```1000000```. Extending that functionality, we could modify the syntax for ```for``` loops and ```if``` statements if we wanted. But there's still an important limitation.

We don't have all the power of the language to define those macros. We just can define some mapping between string patterns and pieces of code. And macros can't make any computation in compilation time. They're just a text substitution, nothing more.

Let's see the real power of Lisp.

## Creating a new language every time

As I said before, when you code in Lisp (in the right way), you are doing two things. Firstly, creating a new language that is specific to your domain, and secondly, coding a program in that expressive and very convenient language you've just created.

Actually, there's no order. You create both the language and the program simultaneously. Both of them evolve during the development process and the final product is a highly readable and clean program.

You can use another brand new operator to create classes that represent stores for example:

```lisp
(define-store my-store (:capacity 100 :budget 100000 :description "My very first store template"))
```

That could be the equivalent to build a class named ```my-store```, which might implicitly inherit from a ```store``` parent class. We are passing a ```capacity```, a ```budget```, and a ```description```.  Then, in compilation time, the ```define-store``` operator can do all the things you can imagine, maybe it is not even a class definition. The thing is, we are declaring an entity in a very clear and domain-specific way.

It might be a toy example and it is totally out of context. But the cool thing is that you can imagine any sort of complicated computations running under the hood, and that will be a possible scenario. With enough expertise, you can transform Lisp into the language you need to make the shortest and cleanest program.

In the above example, the operator ```define-store``` is a macro. A macro in Lisp is like a special function. They can receive parameters and they are executed in compilation time, in a step called _macroexpansion_. Then, the macro call is substituted by its own result in compilation time. For example, we can define the following macro:

```lisp
(defmacro hundred () (* 2 50))
```

We defined a macro called ```hundred``` that receives no parameters. It returns the result of multiplying 2 and 50.

> Notice that we write the arithmetic operator first and the operands later. One of the advantages is that we can make ```(* 2 50 100 200)``` and it will return the product of all those factors. But of course, you can use some advanced Lisp magic to change this syntax ;-)

Now, what happens if we write

```lisp
(+ hundred 30)
```

The first step is _macroexpansion_. The compiler substitutes the macro call for the result of that call and the code becomes:

```lisp
(+ 100 30)
```

Then, the program is executed and we obtain the result ```130```.

Notice the difference with C-like macros. There was more than just text substitution, there was computation and then, code substitution.

If we change the macro definition by adding just one more character (notice the quote):

```lisp
(defmacro hundred () '(* 2 50))
```

Then the _macroexpansion_ changes a little bit too:

```lisp
(+ (* 2 50) 30)
```

But the output remains the same. Now the macro returns the code as-is, and there are no further computations in compilation time.

## Beyond macros

Talking deeper about Lisp macros requires to have a basic knowledge of Lisp programming language. In this article, I don't assume any prior knowledge about Lisp. So, the examples I can use are very limited. It wouldn't make any sense to include a complex macro definition. But my point is not to teach you how to define some macros in Lisp but to show you another way of thinking about programming.

The idea of building your program and the language in which your program is written at the same time is, at least, beautiful. The elegance of the process can only be compared with that of the final result.

We end up with two products. The desired program, but also a language that we can use totally or just partially in our next journey.

Just knowing about this philosophy is totally worth it.

## Conclusions

In this article, I have talked about Lisp's macros.

They are a powerful feature that allows us to define new Domain-Specific Languages for every problem we face. With them, we can create the cleanest programs and get the language in which that program is written at the same time.

That immense power makes us approach programming in a very different way.

This article is part of a series called "Cool things about Lisp". I plan to add more articles to this series in the future. Trust me, there's a lot of more cool things to talk about. Let me know if you like this content and want me to keep writing about it. Leave your impression and/or your comment.

You can follow me on Twitter. I'm always writing about Computer Science stuff.  