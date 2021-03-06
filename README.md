# Callback-Hell
This is a small talk about Node I eventually plan on giving

## What is node? 
```
The following is straight off of the nodejs home page
```
"Node.js is a platform built on Chrome's JavaScript runtime for easily building fast, scalable network applications. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient, perfect for data-intensive real-time applications that run across distributed devices."

Here's a small HTTP server that sends hello world to anyone that pings it. 
```js
var http = require('http');
http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello World\n');
}).listen(1337, '127.0.0.1');
console.log('Server running at http://127.0.0.1:1337/');
```
It's also fast as hell. Because it essentially forces the programmer to multithread as many things as possible. 

## Asynchronous
Speaking of multithreading, thats what nodeJS is. nodeJS uses an event loop to find everything that can be taken apart and run each of those pieces of code as a separate process. This results in code being much faster *by design* because it will end up not working until you do. 

## Callbacks
I am going to try to keep this dead simple. A "callback" is any function that is called by another function which takes the first function as a parameter. 
```js
var myVar = function(var, functionTwo(){
  console.log("this code is in function Two");
});
```

A lot of the time, a "callback" is a function that is called when something happens. That something can be called an "event" in programmer-speak.

Imagine this scenario: You are expecting a package in a couple of days. The package is a gift for your neighbor. Therefore, once you get the package, you want it brought over the the neighbors. You are out of town, and so you leave instructions for your spouse.

You could tell her to get the package and bring it to the neighbors. If your spouse was as stupid as a computer, she would sit at the door and wait for the package until it came (NOT DOING ANYTHING ELSE) and then once it came she would bring it over to the neighbors. But there's a better way. Tell your wife that ONCE she receives the package, she should bring it over the neighbors. Then, she can go about life normally UNTIL she receives the package.

In our example, the receiving of the package is the "event" and the bringing it to the neighbors is the "callback". Your wife "runs" your instructions to bring the package over only when the package arrives. Much better!

This kind of thinking is obvious in daily life, but computers don't have the same kind of common sense. Consider how programmers normally write to a file:

fileObject = open(file)
now that we have WAITED for the file to open, we can write to it
fileObject.write("We are writing to the file.")
now we can continue doing the other, totally unrelated things our program does

Here, we WAIT for the file to open, before we write to it. This "blocks" the flow of execution, and our program cannot do any of the other things it might need to do! What if we could do this instead:

we pass writeToFile (A CALLBACK FUNCTION!) to the open function
fileObject = open(file, writeToFile)
execution continues flowing -- we don't wait for the file to be opened
ONCE the file is opened we write to it, but while we wait WE CAN DO OTHER THINGS!

It turns out we do this with some languages and frameworks. It's pretty cool! Check out Node.js to get some real practice with this kind of thinking.

Let's try a simple code example to get used to what's actually happening. Here's an example of some code using the twitter api to find some information. 

This is a small code sample using `Express.js` to handle routing and serving up content. 

```js
/* when someone goes to 'ourpage.whatever/'  */
app.get('/', function(req, res){
  // pass readTweets the function we want to execute as an argument.
  readTweets(function(tweets) {
    // this function content will be executed inside of the readTweets function
    res.render('index', { content: tweets });
  });
});

function readTweets(callback) {
  // string buffer of tweets
  var tweets = "";
  twitterSearchClient.search({'q':query, 'geocode':city }, function(error, result) {
      if (error){
          console.log('Error: ' + (error.code ? error.code + ' ' + error.message : error.message));
      }
      if (result){
        for(var i=0; i<result.statuses.length; i++){
            tweets += result.statuses[i].text ;
          }
        }
        /* 
        execute the callback function passed to us from the calling function!
        we're really saying
        res.render('index', { content: tweets });
        */
        callback(tweets);
        /* remember the name 'callback' here is arbitrary!  */
      }
  );
}
```

