## The anatomy of an XHR request

The following text is adapted from [Eloquent Javascript, chapter 17](http://eloquentjavascript.net/17_http.html#h_Gh3HVKEFJQ)   

The interface through which browser JavaScript can make HTTP requests is called XMLHttpRequest (note the inconsistent capitalization). It was designed by Microsoft, for its Internet Explorer browser, in the late 1990s. During this time, the XML file format was very popular in the world of business software—a world where Microsoft has always been at home. In fact, it was so popular that the acronym XML was tacked onto the front of the name of an interface for HTTP, which is in no way tied to XML.

The name isn’t completely nonsensical, though. The interface allows you to parse response documents as XML if you want. Conflating two distinct concepts (making a request and parsing the response) into a single thing is terrible design, of course, but so it goes.

When the XMLHttpRequest interface was added to Internet Explorer, it allowed people to do things with JavaScript that had been very hard before. For example, websites started showing lists of suggestions when the user was typing something into a text field. The script would send the text to the server over HTTP as the user typed. The server, which had some database of possible inputs, would match the database entries against the partial input and send back possible completions to show the user. This was considered spectacular—people were used to waiting for a full page reload for every interaction with a website.

The other significant browser at that time, Mozilla (later Firefox), did not want to be left behind. To allow people to do similarly neat things in its browser, Mozilla copied the interface, including the bogus name. The next generation of browsers followed this example, and today XMLHttpRequest is a de facto standard interface.  

It's important that you know that an XMLHttpRequest (XHR) is actually an object that can be accessed by creatin instance of it like this `var xhr = new XMLHttpRequest()`. It has a whole lot of methods available to it in order to interact in different ways with a server.

Here is a simple example of how you can make an XML request:  

~~~
var myRequest = new XMLHttpRequest();
myRequest.onreadystatechange = function() {
    if (myRequest.readyState == 4 && myRequest.status == 200) {
      document.getElementById("demo").innerHTML =
      myRequest.responseText;
    }
};
myRequest.open("GET", "xmlhttp_info.txt", true);
myRequest.send();
~~~

[You can see it in action here](http://www.w3schools.com/xml/xml_http.asp).  

### Line by line
Following is an explanation of each line of code from the block above.

1. `var myRequest = new XMLHttpRequest();` --- this creates a new instance of the XHR object, which you can then use to query a server or file of your choice.  
2. `myRequest.onreadystatechange = function() {...}` --- this method of the XHR object enables you to store a function (after the '=') which will be called automatically each time the readyState property of the myRequest object changes. More here.
3. `if(myRequest.readyState == 4 && myRequest.status == 200){...}` --- every time the readyState property changes, the function will check if these two functions are met:  
  i. `myRequest.readyState == 4` --- the readyState property changes from 0 to 4 as the request is processed. 0 means the request is not initialised, while 4 means the request is finished and the response is ready. However, the response might be that the request hasn't found what it was meant to, which is why we also need:  
  ii. `myRequest.status = 200` --- this property is only valid after the send method returns successfully. It will return a 3-digit status code starting with 1, 2, 3, 4, or 5, indicating what the result of your request was. The code 200 means 'OK', while 404 is notoriously the code for 'Not found'. Full list here, under 'Return Values'.
4. `document.getElementById("demo").innerHTML = myRequest.responseText;` --- this is the code that will run if the request is successful. It generally does something with the response text (which is accessed with myRequest.responseText)
5. `myRequest.open("GET", "xmlhttp_info.txt", true);` --- the open method is important. Here it takes 3 parameters:  
  i. *method*: The HTTP method to use (GET, POST, PUT, DELETE etc)  
  ii. *url*: The requested URL (in this case a local file path: "xmlhttp_info.txt") - in many cases this will be the URL of the server you are querying.  
  iii. *async* (optional): whether the call should be asynchronous or not. false means it waits for a response from the server before continuing execution of the code. The default value is true, which allows you to execute other scripts while waiting for the response. This is generally preferable.
6. `myRequest.send();` --- this method sends the request to the server. Use this after setting up the XHR with the .open() method. If you are GETting, it takes no parameter, but if you are POSTing, it may take a parameter of the string you wish to post.