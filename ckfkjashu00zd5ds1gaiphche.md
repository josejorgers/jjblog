## The test, the bug and the hacker

Certain developer is creating the login page of a random website. When he finishes the work he proceeds to make a login attempt in his brand new page. There are things that don't work properly but he is able to fix them quickly. And finally, all users can enter with their accounts. Let's move this ticket to the Done board.

Once the project was finished it turned out that people couldn't log in with their accounts! Our developer was both stunned and sad at the same time. Luckily this was just a school project and wasn't a bank management software.

Have you experienced such situations? I certainly have. At some point in the development process he made some changes in the database structure, those changes made the login logic to fail. But he was logged in the page already and didn't remember to retest the login page. It was a hard to find error. Damn! 

Does that mean that we are bad at making software? Yes, in a way.

## Bugs are part of the game

Yes, coding that way is certainly wrong. But is not the error itself what was wrong. We are all humans, we make mistakes in every activity we carry on and coding is not an exception. Bugs are part of the game.  

What we need to do is to be prepared to detect that error as soon as possible. We cannot avoid bugs, but we can make a lot of things instead of just login once or twice in order to detect bugs.

>Hope at this point you are motivated enough to keep reading about how to get rid of bugs. But maybe you need to know some examples and figures of how harmful bugs can be. I recommend you to take a look at this  [page](https://www.softwaretestingnews.co.uk/the-real-cost-of-software-bugs/)

## We need to test!

There will be bugs in the code we'll write tomorrow. Why not take care about the "pesticide" today? So, start the project that will test your actual project right now!

There are several ways testing can be carried on, and there are several kinds of tests as well. This post is not about the characteristics of each type of test. Instead I want to make a shallow introduction about the necessity of testing and the right way to test.

Companies that are large enough have entire departments of Quality Assurance (QA) for the software they produce. Testing projects are comparable with the actual product in both length and resources consumption. So yes, you maybe will need to code a lot for the sole purpose of "capturing" your bugs. But it's worth every second and line of code when you are working on a complex enough project.

A bug is harmful only if it finds its way to the released product. The cost of fixing a bug decreases dramatically when it is discovered in an early stage.

Although testing can reduce cost a lot, it seems kind of expensive as well. Yes, I know it is order of magnitude less expensive. But how to make it cheaper? What is the right way to go?

## The way of testing

The first and most important thing to take into account is that testing **can detect the presence of bugs but not their absence**.  So you need to focus your testing on finding bugs, instead of trying to prove the correctness of the software.

In order to be relatively easy to test, your project should be built according with good practices and principles. Actually testing is an effective way to find the best architecture and design patterns. Yeah, you should test even in the earliest stages of the project! Your software should be divided in several modules that fulfill very specific requirements, and we need to be able to test those modules either in isolation or interacting with each other.

We need to automatize as much as we can. And the scripts that contains tests should be light enough to be run lot of times with nearly no cost. Every task that is carried on by humans is prone to errors and is (at least in this context) more expensive. But it might happen that our software would require some test that can't be automated or that would be more expensive if we automatize it, due to the necessity of constant maintenance that it would need, among other possible issues.

But then... Don't we need a test to find bugs on the test itself?

## The snake won't eat its tail

Testing is not bug free. You can certainly introduce some bugs when coding an automated test. Then it seems natural to think about a test that tests the test. But then we'll need a test that tests the test that tests the test. And off course we can come up with the idea that testing is a fruitless effort since it tries to capture some bugs while introducing some others.

Don't worry. I don't like to do fruitless work either, and making this post would be so if that last idea was true. 

I just want to mention another important requirement that every automated test should fulfill: **it needs to be simple**. 

Every complex logic needs to be out of the test and, potentially, be tested by a short, readable and simple piece of code

Thus, test will always be almost error free. Readability is an important feature as well since it allows to find any possible bug introduced in the test very quickly.

## Conclusions

This post is an overview of testing as a necessity in the software development. I approached the subject as an effective way to discover errors during the software construction, which is dramatically cheaper than discovering them when the harm is done. But testing is more than that, it is the most effective way to build good quality software. It should be present in every stage of the development and even be in charge of every stage. Testing is the heart of the software quality assurance.

As important as testing is testing in the right way. So you need your test to be simple, short and readable. That last feature is an important one! Not just because the reason mentioned above but because a test suite can represent the requirements of your product. Yes, you can "define" your product by testing it, but just if that definition can be easily read.  

This post is far from being a deep guide for testing or a treaty about every single aspect of testing. My sole purpose is to motivate you to learn about it. I hope this requirement was fulfilled. Thanks for reading