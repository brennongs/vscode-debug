# An Intro to VSCode Debugging

Visual Studio has a long history of built-in debugging support, and that is no different with VSCode. The debugger shipped with VSCode supports Node.js debugging for server-side code, front end development, Electron desktop apps, and . This walkthrough will show you the basics of the VSCode debugger.

### What is it?

Before we get into how to use the VSCode debugger, let's quickly define what it is, and what some alternatives to it are.<br><br>
Debugging is exactly what it sounds like -- the process of removing 'bugs', or problems, from your code. There are a lot of tools that allow you to do this, some built into the language you're using, others that are 3rd party. The most popular and arguably easiest to learn is the Chrome debugger that comes built in to Google Chrome.<br><br>
Assuming you're reading this in Google Chrome (if you aren't, I strongly suggest you start browsing in Chrome, but that's not what this article is about), go ahead and press <kbd>cmd</kbd> + <kbd>option</kbd> + <kbd>j</kbd> (<kbd>ctrl</kbd> + <kbd>shift</kbd> + <kbd>j</kbd> in Windows). You should see a side or bottom panel expand with what's called the Chrome Debugger Console. <br><br>
![](https://s1.postimg.org/9tsg6agzpb/Screen_Shot_2017-10-11_at_10.19.52_AM.png)<br><br>
This is a fully functioning instance of Node.js that can be used to interact with the web page it is opened on. For example, in your code, when you define a `console.log()`, this is the console it logs to. You can also type `debugger` to create what's called a 'break point' in your code, which tells Chrome to pause rendering so you can inspect your code. Essentially, whatever you do with JavaScript in your IDE, you can do in this console.
