# An Intro to VSCode Debugging

Visual Studio has a long history of built-in debugging support, and that is no different with VSCode. The debugger shipped with VSCode supports Node.js debugging for server-side code, front end development, Electron desktop apps, and tons more. This walkthrough will show you the basics of the VSCode debugger.

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

### Configuration -- Node.js

First thing's first, every project that you are going to set up to use VSCode debugger needs it's own `.vscode` directory with a `launch.json` file in it. I've included an example of the finished product of that file, set up to run both debugging configurations seprately and together, so you can refer back to it as we walk through building our own. There are some properties that are defined in the file that are specific to this process, and I'll define those as we go. For right now, go ahead and make your `.vscode` directory. It's possible you already have one depending on what extensions you're using. Either way, inside `.vscode`, make a `launch.json` file and open it in VSCode.

The nice thing about this being a built-in tool in VSCode is that VSCode will do a lot of the work for you. 

![](https://s1.postimg.org/1ddmjqss0f/Screen_Shot_2017-10-11_at_1.46.49_PM.png)

In the bottom right corner of the editor workspace, click on the button that reads "Add Configuration...", and then hit <kbd>enter</kbd>. This will produce the basic configuration with none of the properties defined.

![](https://s1.postimg.org/1fjgul5qa7/Screen_Shot_2017-10-11_at_1.53.57_PM.png)

Notice that the "Add Configuration..." button didn't disappear? That's because we can click it again, and when we do, VSCode will give us a big ol' list of Node debugging configurations. We'll click "Node.js: Launch Program", and violà, we have our first configuration.

![](https://s1.postimg.org/639xxf8gwf/Screen_Shot_2017-10-11_at_2.18.06_PM.png)

The code that appears looks like this:

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "node",
            "request": "launch",
            "name": "Launch Program",
            "program": "${workspaceFolder}/app.js"
        }
    ],
    "compounds": []
}
```
The `type` property tells VSCode what kind of debugger to run. `request` defines what to do with the debugger. `name` is how you reference this configuration later on, and `program` is the path to your root file. For example, in a full stack project, if your server file is in `server/server.js`, you'd update the `program` path to `${workspaceFolder}/server/server.js`.

### Application

Now our debugger is set up to debug a Node.js server. Hit <kbd>cmd</kbd> + <kbd>shift</kbd> + <kbd>d</kbd> to open the debugger menu. At the top, select the play button. Throw a `debugger` in an endpoint and send a request to that endpoint. The code pauses, and if we hover over a variable we can see its value. Hover over `req` and you can see the entire request object!

![](https://s1.postimg.org/7lrjx6fthb/Screen_Shot_2017-10-11_at_4.35.08_PM.png)

This can also be accomplished by clicking to the left of the line number for the line you want to pause on. You will see a red dot appear, and when you run your code it will respond the same way.

### Configuration -- Chrome

Let's take a look at how to use VSCode debugger with Google Chrome. For this one you'll need an extension. Hit <kbd>cmd</kbd> + <kbd>shift</kbd> + <kbd>x</kbd> to open the extensions menu, and search "debugger chrome". The first hit is the correct one. Click the green `install` button and reload the window.

![](https://s1.postimg.org/51mg9era33/Screen_Shot_2017-10-17_at_1.04.10_PM.png)

Now open your `.vscode/launch.json`, click "Add Configuration..." and you'll see that we have two new additions to our list: "Chrome: Attach", and "Chrome: Launch".

![](https://s1.postimg.org/9tji6ojif3/Screen_Shot_2017-10-17_at_1.09.23_PM.png)

#### Attach & Launch

We see the same options for our Node config -- "Attach" and "Launch". They mean just what you'd think. "Attach" will run the debugger along with an already running `debug` instance of your code, whether that's Node or front-end code through Chrome. "Launch" starts a new `debug` instance with whatever tools you'll need.

Now, it's pretty easy to run your back end in `debug` mode -- simply run `node path/to/your/server.js --debug`. But what about Chrome? Chrome is the render tool for our front end code, and in order to attach our VSCode debugger to it, we'll need to first determine how to run Chrome in `debug` mode.

The answer requires your terminal. You'll need to run this command:
```bash
$ sudo /Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --remote-debugging-port=9222
```
I've set this to an alias `chrome` in my `~/.bash_profile`, so when I want to debug front end code it's just a matter of six keystrokes to start up the right Chrome. The reason I've done so is that I have yet to figure out how to run VSCode Debugger unless it's an "Attach" request. /shrug.

### Application

That said, click "Add Configuration..." and select "Chrome: Attach". Now when we start up our app, we can select the "Attach to Chrome" option in the debug dropdown (<kbd>cmd</kbd> + <kbd>shift</kbd> + <kbd>d</kbd>), click the play button and the debugger will start. **If it doesn't work, try restarting your terminal script.** 

![](https://s1.postimg.org/5eaa9fv2db/Screen_Shot_2017-10-17_at_5.04.18_PM.png)

Throw in a breakpoint by clicking to the left of a line number and refresh your Chrome page.

![](https://s1.postimg.org/4wk8kv46vj/Screen_Shot_2017-10-17_at_4.51.00_PM.png)

Chrome will tell you that Visual Studio Code paused the code, which I think is the dopest. You can interact with the code in the same way as when you debug your Node server. Hover over variables to see their definitions, you can interact with the code with Quokka if you have it installed. The world is your oyster.

### Hybrid debugging

So this is cool, but what if we could do both at the same time?

![](https://s1.postimg.org/2vkrfawr7j/Screen_Shot_2017-10-17_at_5.13.55_PM.png)

See the `compounds` property? This will run two debugging processes concurrently. \*Mic drop\*

The `configurations` property is an array with the names of the two configurations you want to run, and `name` is just a string. In the debugger menu drop-down select whichever option mirrors what you called your new compound and click the play button. Now you can string your breakpoints all the way through the stack. Send a request from the front end, make sure that it does what it needs to before it gets passed to your server. Then make sure it successfully gets inserted into the database.

Welcome to the future.

### Additional Resources

[Actual Microsoft Presentation on VSCode Debugger](https://youtu.be/UcW1FHNvy8M)

[VSCode Debugger for Chrome Docs](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code)

[VSCode Debugger for Node.js Docs](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)

[Microsoft Docs for general debugging in VSCode](https://code.visualstudio.com/docs/editor/debugging#_debugger-extensions)

