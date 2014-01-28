showModalDialog sample Web Application for all browsers (IE,FF,Chrome)
=============================

This is just a small demo of JavaScript code that will let you edit sample data in a modal window (only some demo workflow to show how things work). Purpose of the demo is to show working solution for IE5-IE11 (tested by switching the Document mode in developer tools and FireFox 26 and Chrome 34 canary.
Sending data
-
In the app I use
```JavaScript
window.showModalDialog
```
to open a modal window and I am passing a JSON object as second parameter with data, that modal window and it's page will need.

I would strongly advise you to use JSON object (in server side analogy with any DTO POCO/POJO object) to decouple pages (you can view them as instances of classes with private properties), since markup can change and this change would need to be propagated to all modal pages everywhere. Also browsers try to shield you from accessing DOM in previous page. Of course you can pass pointer to document object itself as parameter and edit/manipulate it as needed/desired, but a better approach would be to pass just data (its like passing this to another page, or pointers to private properties in C++, just a point of view).

Reveiving the data
-
Calling
```JavaScript
window.showModalDialog 
```
will pause JavaScript on the host page and let you continue on the modal one where we are passing back another JSON object containing data with help of
```JavaScript
window.returnValue
```
That can be anything, in our case values from inputs on the modal page. (Small note here : Chrome implemented openModalDialog in a bit different way since it's not modal and you can navigate away - but it will reain open and JavaScript waiting for object to be passed back will be paused until you close the window and only then it can continue).

Then JavaScript on original page continues with checking the return values and filling in inputs as needed.   

This sample code was tested on Windows 7 SP1 with IE11 by setting the Document modes from edge (11) down to 5 and with latest versions of FireFox - currently 26 final and Chrome 34 canary - beta.

Small note here :
=
I was also testing another code that seems to work, but only in non IE browsers and that is :
```JavaScript
window.opener.document.getElementById('someID').value = 'some value';
```
If you are targeting everything except IE than you are fine, if not, code above may help you.
The future :
=
How long this will work, that is a good question, because showModalDialog is IE specific stuff and FF and Chrome just had to implement it to make the switch from IE possible for users. But currently Chrome 34 is complaining that : 

Chromium is considering deprecating showModalDialog. Please use window.open and postMessage instead.

In this case, postMessage doesnt have to be used, since all of pages are ours, but you would have to use them, if pages would be from different domains.

More on this here : [http://blog.teamtreehouse.com/cross-domain-messaging-with-postmessage](http://blog.teamtreehouse.com/cross-domain-messaging-with-postmessage "postMessage")  