# Callback-Hell
This is a small talk about Node I eventually plan on giving

## What is node? 
Node.js is a platform built on Chrome's JavaScript runtime for easily building fast, scalable network applications. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient, perfect for data-intensive real-time applications that run across distributed devices.

It's fast as hell. Because it essentially forces the processor to multithread as many things as possible. 

## Asynchronous

## Callbacks
I am going to try to keep this dead simple. A "callback" is any function that is called by another function which takes the first function as a parameter. A lot of the time, a "callback" is a function that is called when something happens. That something can be called an "event" in programmer-speak.

Imagine this scenario: You are expecting a package in a couple of days. The package is a gift for your neighbor. Therefore, once you get the package, you want it brought over the the neighbors. You are out of town, and so you leave instructions for your spouse.

You could tell her to get the package and bring it to the neighbors. If your spouse was as stupid as a computer, she would sit at the door and wait for the package until it came (NOT DOING ANYTHING ELSE) and then once it came she would bring it over to the neighbors. But there's a better way. Tell your wife that ONCE she receives the package, she should bring it over the neighbors. Then, she can go about life normally UNTIL she receives the package.

In our example, the receiving of the package is the "event" and the bringing it to the neighbors is the "callback". Your wife "runs" your instructions to bring the package over only when the package arrives. Much better!

This kind of thinking is obvious in daily life, but computers don't have the same kind of common sense. Consider how programmers normally write to a file:

fileObject = open(file)
# now that we have WAITED for the file to open, we can write to it
fileObject.write("We are writing to the file.")
# now we can continue doing the other, totally unrelated things our program does

Here, we WAIT for the file to open, before we write to it. This "blocks" the flow of execution, and our program cannot do any of the other things it might need to do! What if we could do this instead:

# we pass writeToFile (A CALLBACK FUNCTION!) to the open function
fileObject = open(file, writeToFile)
# execution continues flowing -- we don't wait for the file to be opened
# ONCE the file is opened we write to it, but while we wait WE CAN DO OTHER THINGS!

It turns out we do this with some languages and frameworks. It's pretty cool! Check out Node.js to get some real practice with this kind of thinking.
