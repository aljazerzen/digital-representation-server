# Abstraction

> TLDR; almost every online service implements a few mechanisms such as user authentication. It seems reasonable to define protocols that could handle these mechanisms, so the responsibility of their implementation, maintenance and improvement could be shifted from online systems to the protocols and they could provide greater user experience compared to an average current implementation.   

Almost all websites and online systems consist of several "parts" that can be seen across the internet. 

For example, a simple web store selling some home-made products consists of:
- page(s) presenting the merchant & their store
- catalogue browser
- registration & login mechanism
- my cart & purchase
- communication with the merchant

Similarly hair saloon has:
- page(s) presenting the saloon
- registration & login mechanism
- booking area

And online hosting provider has:
- page(s) presenting the company & service they provide
- pricing pages(s)
- registration & login mechanism
- payment
- dashboard for using the service

Social media/streaming site:
- page(s) presenting the company & service they provide
- registration & login mechanism
- (payment)
- actual site providing the service

It can be easily seen that these sites have some common parts. For example registration & login mechanism is present on all examples. Following common sense, these parts should be implemented only once and every site would be using that implementation. 

Before doing such a thing, we must consider whether it is cost effective to do this on not. After all, login & registration is a very basic idea, right? When you think about it, actually no, it is not. If using standard email & password authentication mechanism, sites have to implement safe storage of the credentials, email verification and password reset mechanism. If the site is deemed to have higher security, it should also implement multiple factor authentication (SMS, notification or some other login) and some form of detection of suspicious logins. 
From the user's point of view, authentication systems could also benefit from some standardization. Different password requirements, a lot of passwords to remember and insecure or slow implementations of the mechanism are something we are all familiar with. 

It is obvious that we would all benefit from "shift of implementation of the authentication mechanism", so the first question is how would that work in our case of the sites distributed across the internet?

## Idea of abstraction

At some point in human history, we have started counting. Primarily for practical reasons of keeping track of stored food, trading or agricultural planning. And with counting came basic arithmetics needed to answer some basic questions:

- if we had 15 cows and got 8 more, how much cows do we have?
- if I have 10 bowls of grains and plant them, and every bowl produces 3 bowls of grains, how much bowl of grains would I have? 

For us is now obvious that when adding two quantities together, one can disregard *what* they are adding and focus only on *how much* there is. It doesn't matter if you are talking about cow or bowls of grain, 15 + 8 will always be 23. This may not be obvious to an uneducated mind, but brings great advantages. You can develop arithmetics for numbers and apply them to whatever unit you want. One can discover exponential and logarithmic arithmetics only once and applies it to both cows and for bowls of grain (and every other countable thing).

But it goes further. Linear algebra is branch of mathematics concerning vectors spaces, that defined a lot of operations, theorems and observations that can be used then dealing with vectors and matrices. If you are unfamiliar with vectors, you can think of them as "a few numbers stacked in a column". They can also be represented as a direction in space with a magnitude. And if you think about it (and mathematicians have), they can be represented as polynomials (some sort of function). If you ask a mathematician what vector actually is, they will tell you that it can be any thing that has 'multiple components properties'. Of course linear algebra defined this properties in detail, but the interesting part is, that it can be applied to *anything* with those properties. It has abstracted dealing with "multiple component things" away from numbers.

In essence mathematics is just abstracting ideas away from actual problems, solving it in general and then applying it back to the problems. This is good, since abstracted problem has usually been solved already or you have some other tools that help you solve it.

## Programming languages

To all the programmers reading this it is obvious what I am talking about: inheritance in object oriented programming. When you have a bunch of objects with common properties or functions, you can create the general, abstracted object. For example, when creating a UI, usually there are buttons to be programmed. All of the buttons should have a gray border, should be rounded and show an animation when clicked. So you create such an object and every time you need a button, you just use the one you have already created. But sometimes you would also need it to have a black background, so you create a new child object extending the original one. Now the child inherits all properties of the original button but can also define its own. This approach has the following advantages:
- usually there is less code to write, to have bugs and to maintain 
- when requirements change and all the buttons should not be rounded, you can just change the original button and the change will affect all of its descendants. This also goes for when there is bug with the button animation that has to be fixed.
Of course there are disadvantages. Bad implementation can be overly complex and even harder to maintain, since changes of an object can influence large number of other objects.

This idea of "implementing once, using twice" is not present only in OOP, but is basically what you do when writing functions. Do you have a chunk of code that you would want to be executed more than once, at different parts of your program? Should it change its behavior in accordance to some parameters? This is when you write a function.

## Networking

But what about programs distributed across different systems? Has such an abstraction been done before? Yes, it has. A lot. Enter ISO OSI model.

For simplicity sake, forget about first 3 layers, since they are implemented by your OS and focus on what application has to perform to complete these two tasks:
Request a HTML file
- resolve domain name to an IP address
- compile a request for the file
- segment it into pieces IP can transmit
- check the recipient if they can receive the pieces
- send the pieces to the recipient
- make sure that you do not send them too fast but also not too slow
- make sure to resend a piece if it gets lost on the way
- wait for the response pieces
- message the sender that you got the pieces
- compose the pieces back together

Send an email (by your email server)
- resolve domain name
- segment email into pieces
- check the recipient if they can receive the pieces
- send the pieces to the recipient
- make sure that you do not send them too fast but also not too slow
- make sure to resend a piece if it gets lost on the way
- wait for the recipient to tell you that they got the mail

A lot steps repeat in both cases, so let's group them into one step and call it TCP. Is should be able to open a connection that could send stream of data of any size and make sure it all gets there.

Let's rewrite steps for our two tasks using TCP.
Request a HTML file
- resolve domain name to an IP address
- compile a request for the file
- send the request via TCP
- wait for the response via TCP

Send an email (by your email server)
- resolve domain name
- send the mail via TCP

## Conclusion

All of these examples of abstraction seem obvious and intuitive and are only reasonable thing to do, because:
- solution of the abstracted problem can be applied to multiple actual problems
- abstracted problems are usually more transparent and thus easier to solve
- improvements to the solution of the abstracted problem are also applied to all actual problems

Disadvantages come in the form of:
- solution of the abstracted problem may be very general and too large for some actual problems
- vulnerability of the solution of the abstracted problem is also inherited by all actual problems

> TODO: discuss password managers as naive implementation of what we are trying to achieve
