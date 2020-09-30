## My first year as a developer

It have been a year since I started to make money from building software. I don't call myself a developer, maybe one year making a living from programming is very similar to your definition of what developer is, but I don't see the things in that way. However this could be the subject of another post. Here I will write about some of my first year experiences.

>I'll include some side notes like this one. I'll use these notes to briefly introduce some concepts and tools for those who are starting their programming journey right now.

## Rigidness is not that bad

When it comes to choose a framework or a library there is often a lot of discussion. At some point in my evolution as a programmer I thought that flexibility is always good. After a bunch of lines of code I learned that rigidness is not that bad.

Frameworks, libraries, and tools in general are here because we are bad programmers by nature (among other causes). I really appretiate when a tool force me to make things in a unique specific and very good way, so I can acomplish my goals by following that way.

In a smooth scenario, when definitions and requirements don't change abruptly during a week I always choose rigidness. I always choose the straight path.


## When premises don't hold

Then here I am, a guy that prefers the straight path in the middle of this jungle with no path at all. Definitions and requirements are changing every day in the most abrupt possible way and every week I am doing a very different project. I had to learn to embrace flexibility, in the hard way.

Now I am going to ilustrate that hard way through examples.

They decided to build a website using React. I learned React while doing the project so I started to consume a lot of tutorials. I decided to use ```create-react-app``` (CRA) to scafold the project since it is used in every single tutorial. So far, so good. I had a good workplace and was able to make the production build with a single command and so on.

>I highly recommend you to learn something while doing a project and, if possible, while getting paid for it. I think it is the best way to learn, at least when it comes to frameworks and tools in general.

>CRA is a very useful way to get started with your React project. With just typing ```npx create-react-app <project-name>``` you get the whole project scafolding. I think every React developer can take advantage from this tool no matter if you are novice or senior. I won't complain about CRA, but I think some tutorials should warn us about the possible drawbacks resulting from using it.

Then the boss learned what a Progessive Web App (PWA) is, and after a couple of weeks we were required to make the site progressive. This is not a big deal since CRA provides us with a service worker.

>PWA is a mechanism to make websites working offline and to behave like native apps in mobile devices, among other benefits. This is achieved with the colaboration of several components like the browser, a special script called service worker, a manifest file, and some code to install the service worker in the browser. As I said, CRA provides us with the required components in order we can make our React site progressive.

Once you have a service worker in your site, you are able to receive push notifications from a server, and of course, after two weeks we were required to implement this too. But to make that possible you need to make some adjustments in the service worker and guess what, the CRA service worker is not customizable at all!

>Maybe you have encounter some sites that ask you whether you'd like to receive notifications. If you answer yes, they can send you the so called push notifications with the content they like. This notifications pop up in your device no matter that you have the browser closed. This is possible because the browser keeps the service worker script running in the background.

Then I had to build a new service worker from scratch and the code to install it. That is not a big deal because I had have to do it no matter I had used CRA or not. So let's move to other issues.

After building the whole payment process, they decided that we needed to use some service that verifies your identity from some images of your face and of an indentity document. So we needed to change a lot of things in the already built payment process. I made possible that the user could take a selfie, and take the picture of his ID document. But after that, we were required to use an specific SDK provided by the service in order to make the images to fulfill some features required for a reliable evaluation.

The SDK was a vanilla Javascript file that needed to be imported via HTML script tag. It is intended to be integrated in applications that use vanilla javascript. You know, the non-NodeJS one. So I needed to make the impossible to get everything working without making ```eject```. After knowing about webpack, I'd like to go back in time and start my project from scratch, without using CRA or anything and taking care of my own webpack configuration. Belive me, this is just a single example because I don't want to make the story so long, I just want to make my point.

>Webpack is a tool that allows us to translate our NodeJS code into a vanilla javascript bundle that can be used in our production site. It's hard to master it and that's why tools like CRA manage all the webpack related issues for you. But I haven't found a smooth enough way to make changes in the CRA webpack configurations, so I think that if you are facing the sort of complexities I am telling you about, you should consider using some more flexible tool (and then tell me what tool is that) or doing your own webpack configurations. CRA allows you to run ```npm eject``` and after that, you are by your own with webpack and everuthing, but I am not brave enough yet.

My first task in this company, before the React website I have written about above, was to build a responsive website. After a lot of effort and passing through all sort of dificulties, the site have never been used. Yes, I got paid, but this post is not about making money, this post is about what I think is wrong.

## What I would do

The problem here is not the changing. There are always unstable scenarios, business that are unstable by nature, or at least that becomes unstable due to new events. The problem is the rush in the decision making, the lack of connection between developers and the business men and the misconception by some of those business men that the cost of developing is negligible.

I think you just learn about good programming practices when you find yourself repeating the same code everywhere and fixing the same bug in diferent places. Well, with this experience I have learned the necessity of planning and writing. You need to define your problem and to write down that definition. You need to keep a record of your requirements, and even keep a record of the infrastructure you need in order to fulfill those requirements.  I don't like to overthink, but it's just a lack of common sense thinking that you can get something by just hitting the keys when the project is complex enough.

The problem is not the need to make big changes in the project per se. But if you can include those changes in the initial plan then they are not changes but initial requirements, and the developing process is faster, cleaner and no one do fruitless job. The sooner the change is predicted, the smaller and the easier the refactoring.

## Conclusions

Those have been some of my experiences in this firs year as a developer. I'll certainly write about some others, don't think everything have been that painful.

I hope you or your boss don't be suffering the WIPCA syndrome (Why Isn't Paul Coding Anything?!). So always try to write your problem definition, the requirements and everything you think is necessary, like the infrastructure. No matter if you are using Scrum, Waterfall or whatever. Just write a little bit and try to make changes, which will always be needed, less painful. Of course this only apply to complex enough projects, but don't underestimate projects, actually a way to assert whether a project is complex enough is defining the problem and the requirements.

I you liked this post hit the thumb up. Feel free to comment whatever you want. You can also follow me on Twitter for debates about Computer Science.