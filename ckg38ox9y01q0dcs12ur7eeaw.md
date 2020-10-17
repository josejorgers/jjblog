## Building an E2E Test Framework using some good Design Patterns

End to End (E2E) testing is about simulating the user experience. It doesn't deal with functions, variables, classes, or databases, instead, it deals with buttons, clicks, expected messages, links, etc. We can say that E2E testing is the "ultimate" testing since it checks whether the product as a whole behaves as expected.

In general, E2E testing is difficult to automatize. First of all, we need tools that can interact with the application that is been tested, we need to fill forms, wait for a page to load completely, and that kind of stuff. We also need to get the results from the user interface, we don't have functions returning objects but HTML elements containing the information. Mocking a real user can be challenging and might require a lot of maintenance. In this post I will talk about my own experience building an E2E Testing Framework, I applied some cool Design Patterns so I think this could be interesting for you even if you have nothing to do with E2E Testing Automation.

This post is _language and tool agnostic_, it means that I won't refer to a specific programming language nor a specific E2E tool like Selenium, Puppeteer, or Playwright. By the way, those are great tools for automatizing E2E tests. Furthermore, this post focuses on E2E Testing for websites.

## The Problem

I was required to design a framework to perform different E2E tests on different websites. More precisely we needed to make some tests over specific React components inside those websites. Every component has the same classes and id's no matter the website and just changes slightly from one site to another. We needed to make tests for every possible viewport (mobile, tablet, and desktop), and the components change their structure when the viewport changes.

Furthermore, we know nothing about the developers. Thus, I needed to be prepared to manage some unforeseen changes in the interface relatively easy. In other words, one critical requirement the framework needed to fulfill was to be easy to maintain.

How to make an E2E Testing Framework that doesn't care too much whether developers changed the id attribute of some button that is clicked in some test? How to be able to write tests for some component that is not created yet? And, if possible, how to make every test easy to read and understand?

All those goals can be achieved by applying some abstractions and design patterns. Here we go!

## The Page Object Model

The first thing we need to do is to create an abstraction for a page. This is important for several reasons. It will increase readability. For example, you don't want to have a line in your test that reads ```tool.getByCssSelector("button.btn.btn-submit").click()```, instead you want to have a line like this one: ```page.clickSubmitLoginFormButton()``` or something similar. You also need to keep all the CSS selectors and DOM related stuff in a single place, this way, when something in the interface is changed you only need to modify one single file (or maybe two, but not more ;-) ).

That abstraction is called the **Page Object Model**. You make a class that represents only the elements that you are interested in from the page. You put all the DOM related stuff in those classes.

In my case, I did it slightly different. I created two classes for every page. A **PageModel** and a **Page Object**. In the first one, I put the elements of the page. For example, suppose we are testing a login page, then my **LoginPageModel** would be like:

```pseudocode

class LoginPageModel

    constructor(tool)

        this.tool = tool


    loginUsernameInput()

        return this.tool.getById('username-input')


    loginPasswordInput()

        return this.tool.getById('password-input)


    loginSubmitButton()

        return this.tool.getById('submit-login-button')

```

If any of those element changes in the future we only need to modify the corresponding **PageModel** class.

In the **PageObject** class, I add the actions that you can perform on the page. An example of a **LoginPageObject** class would be:

```pseudocode

class LoginPageObject

    constructor(pageModel)

        this.model = pageModel


    typeUsername(username)

        this.model.loginUsernameInput().type(username)


    typePassword(password)

        this.model.loginPasswordInput().type(password)


    clickLoginSubmitButton()

        this.model.loginSubmitButton().click()

```

Here we can get advantage of a statically typed language that can get all the methods of the model class in compilation time, that way some IntelliSense tool can remind us the name of every method that represents a page element, and we also get more compilation errors and fewer runtime errors, which is very good for us and our mental health.

Why to separate page elements from page actions? A single class that contains both the elements and the actions can be very large. We can say that by doing this we are applying the Single Responsibility Principle and that would be cool, but in this case, that has not too many practical significance beyond readability and keeping classes simple.

With the **Page Object** abstraction we can make tests that only depend on page objects instead of writing some tricky CSS selectors in the middle of the test code. We keep all the DOM related stuff in a single place and our tests can be more expressive and easy to understand.

## Tests. The Facade Pattern

Now we have many classes that contain all the elements and actions of several pages. What we need to do now is to build our tests. Those tests will provide a simple interface that exposes to the client the ```run``` functionality that returns a test result. So the client doesn't have to worry about accessing any element or doing any action, just needs to instantiate the test and run it.

When we provide a simple interface that hides a more complex infrastructure we are applying the Facade Pattern. I know that's only a fancy name for something that is clear we needed to do. Continuing with our Login Page Test example, the **LoginTest** would be something like this:

```pseudocode

class LoginTest


    constructor(loginPageObject)

        this.pageObject = loginPageObject


    run()

        this.pageObject.typeUsername("TestUser")

        this.pageObject.typePassword("TestPassword")

        this.pageObject.clickLoginSubmitButton()

        assert that the login was successful
```

The last line of the ```run``` method is an assertion. Depending on the complexity of the assertions you use, you can either define them separately or inside the Page Object. By choosing the first option you can reuse and extend assertions, but if your assertions are very specific for each case and simple enough the first option can be overkill and you probably will be good with the second one.

We are also injecting the Page Object dependency in the Test. We are not doing ```this.pageObject = new LoginPageObject()``` but receiving the dependency as an argument in the constructor. That way we can instantiate the same test for another page. We also inject the Page Model in the Page Object instances, then we can have the same Page Object with another model (eg: same LoginPageObject instance with a LoginMobilePageModel instead of a regular LoginPageModel).

But now, to instantiate a test, we need to instantiate one or more Page Models, then one or more Page Objects, and finally the Test. It seems too hard. That's precisely one of the drawbacks of using Dependency Injection, but it is solvable!

## The Factory Pattern

If it's difficult, let's delegate the responsibility to another abstraction. In this case, we'll make some Factories. These are classes that are used to instantiate other classes. In this case, every Factory class will be responsible for instantiating a specific test. That's the Factory Pattern in action!

So we can create a **LoginTestFactory** for our LoginTest:

```pseudocode

import tool

class LoginTestFactory


    create(config)

        if config.viewport == 'mobile'
            then return new LoginTest(new LoginPageObject(new LoginMobilePageModel(tool)))
        else
            return new LoginTest(new LoginPageObject(new LoginPageModel(tool)))
```

Here we are representing with ```tool``` any possible technology you could use to get the elements of a page and interact with them. Maybe you don't pass the imported tool as is, but you create some objects using that tool and then pass those objects as parameters. But the idea is that all the relatively complex logic to make an instance of a Test is encapsulated in a Factory object.

To run our test we only need to do something like this:

```pseudocode

runLoginTestDesktop()

    factory = new LoginTestFactory()

    config = new ConfigObject(viewport = 'desktop')

    test = factory.create(config)

    test.run()



runLoginTestMobile()

    factory = new LoginTestFactory()

    config = new ConfigObject(viewport = 'mobile')

    test = factory.create(config)

    test.run()
```

Now, in the Conclusions section, we'll check whether we have accomplished our initial goals

## Conclusions

Building our framework in the way I have shown you in this post, can dramatically decrease the cost of a change in the user interface. All the code that depends on the user interface is isolated in specific classes that abstract the concept of a page.

That abstraction also allows us to  write the tests for the next week! I mean the test for components that have not been created yet. We just make the required new PageModels and PageObjects to mock the elements on the page that will be created and we can build the rest of the process in the same way we have seen so far. When we had the specific elements on the interface we can change the page models and verify whether the application behaves as expected.

We also have tests that are very easy to read and understand since we make expressive actions like ```this.pageObject.clickLoginSubmitButton()```. Thus, our tests can describe the requirements of our application and can be easily maintained.

E2E testing automation is difficult because is difficult to keep it simple, and a complex test is not a test. In this post, I have shown some design patterns and good practices to make it smoother. I have tried to make it language and tool agnostic in order you can apply these practices in your project no matter what language or technology you are using. I only assumed an Object-Oriented programming language.

No matter if you are not making an E2E Testing Framework, I made this post because I think some of these tricks can be applied in a relatively wide variety of problems.

Feel free to comment any suggestion, impression, or doubt about this article below. You can also follow me on Twitter for debates about related topics.
