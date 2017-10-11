# An Intro to VSCode Debugging

Visual Studio has a long history of built-in debugging support, and that is no different with VSCode. The debugger shipped with VSCode supports Node.js debugging for server-side code, front end development, Electron desktop apps, and . This walkthrough will show you the basics of the VSCode debugger.

*For this walkthrough, I assume you already have a project that you want to try this on, so I haven't provided an example project.*

## Why do we need it?

Before we get into how to use the VSCode debugger, let's quickly define what debugging is, and one major alternative to VSCode debugger.


Debugging is exactly what it sounds like -- the process of removing 'bugs', or problems, from your code. There are a lot of tools that help you to do this, some built into the language you're using, others that are 3rd party. The most popular and arguably easiest to learn is the Chrome debugger that comes built-in to Google Chrome.


Assuming you're reading this in Google Chrome (if you aren't, I strongly suggest you start browsing in Chrome, but that's not what this article is about), go ahead and press <kbd>cmd</kbd> + <kbd>option</kbd> + <kbd>j</kbd> (<kbd>ctrl</kbd> + <kbd>shift</kbd> + <kbd>j</kbd> in Windows). You should see a side or bottom panel expand with what's called the Chrome Debugger Console.


![](https://s1.postimg.org/9tsg6agzpb/Screen_Shot_2017-10-11_at_10.19.52_AM.png)


This is a fully functioning instance of Node.js that can be used to interact with the web page it is opened on. For example, in your code, when you define a `console.log()`, this is the console it logs to. You can also type `debugger` in your IDE to create what's called a 'break point' in your code, which tells Chrome to pause rendering so you can inspect your code. You can also draft functions and variable declarations here, as well as view your code as it is processed by Chrome.


This is a super useful tool. Most browsers come with some version of a debugger built in, and I've found Chrome's to be extremely easy to use, but it still requires hopping between your IDE and browser to find problems in your code. And, even worse, if you're debugging server-side code, `debugger` works quite differently, and your `console.log()` logs to whatever terminal window is running your node process. That's a minimum of three windows, just for development, on top any other windows for research or team communication or whatever you may be using at the time. That's too confusing. Enter VSCode to solve all your problems. :)



## How do we use it?

So now we know what problem we're addressing, and how the problem has been addressed so far. Let's take a look at how to actually use VSCode debugger. We'll see how to use the tool to debug our server-side code first, then how to use it in conjunction with Chrome. Then we'll look at how to use both configurations to debug a full-stack application at the same time. (That's only one window!)

### Configuration

First thing's first, every project that you are going to set up to use VSCode debugger needs it's own `.vscode` directory with a `launch.json` file in it. I've included an example of the finished product of that file, set up to run both debugging configurations seprately and together, so you can refer back to it as we walk through building our own. There are some properties that are defined in the file that are specific to this process, and I'll define those as we go. For right now, go ahead and make your `.vscode` directory. It's possible you already have one depending on what extensions you're using. Either way, inside `.vscode`, make a `launch.json` file and open it in VSCode.

The nice thing about this being a built-in tool in VSCode is that VSCode will do a lot of the work for you. 

![](https://s1.postimg.org/1ddmjqss0f/Screen_Shot_2017-10-11_at_1.46.49_PM.png)

In the bottom right corner of the editor workspace, click on the button that reads "Add Configuration...", and then hit <kbd>enter</kbd>. This will produce the basic configuration with none of the properties defined.

![](https://s1.postimg.org/1fjgul5qa7/Screen_Shot_2017-10-11_at_1.53.57_PM.png)



