# LightTable Plugin guide

A guide to building plugins in LightTable.

## Introduction

Lightable plugin development in a nutshell:

    Command LT to do something
    Command executes and raises Triggers
    Triggers are captured by one or many Behaviors
    They listen to them from a pool
    Once they capture a trigger
    They respond with a Reaction    
    Reaction is just an anonymous function
    that bares no access to the outside
    But enclosed in THIS
    THIS is Lighttable
    Sharing I/O
    Realtime
    
This introductory chapter shows you how to download, run, and modify a light plugin from source. Once we figure out how things are put together in Lighttable, we can create new plugins.

### Pre-Requisite

Make sure LT_HOME is setup on your ENV: start Lighttable via command line:

    light

This way, Lighttable can access your ENV, run a unix command and pipe its output into Lightable's reactive surface. 

For this guide, we'll study [lt-gist](https://github.com/sreeharsha-m/lt-gist), a plugin written as an exercise. To run this plugin you need:

     `gem install gist`.

**lt-gist** is a simple plugin that gists your InstalRepl (or file), anonymously.

### Install a plugin

`Ctrl-Space` and start typing: `Show plugin Manager`.

> `Ctrl-Space` opens the command navbar and shows available commands. Start typing in the command navbar to filter your list.

Select `Available` and browse for `Gist`. 

`Install`.

Create a new file and save it as `.clj`. Write a _hello world_ or something simple. 

Save.

`Ctrl-Space` and start typing ... `Gist`, select it.

Open a new tab and hit paste (from clipboard).

Congrats, you just gisted your code anonymously, from Lighttable!

### Setup keybindings

**Short answer**: 

`Ctrl-Space` and start typing `Settings: User keymap`, select it. 

In the tab that just opened, under the `:editor` map of existing keys shown in `{}`, paste the following key value pair: 

    "ctrl-shift-g" [:gist.submit]
    
### Thinking in Commands, Triggers, Behaviors, and Objects

The first step of customizing your plugin is binding your keyboard shortcuts to commands. In the plugin's source code, grep for `(cmd/command`. It's just a map with a few keys. Look for the value of `:command`. In `lt-gist` it's [:gist.submit](https://github.com/sreeharsha-m/lt-gist/blob/master/src/lt/plugins/gist.cljs#L41).  

> `:command` points to another key, an element in a set of Triggers, composed in a Behavior ready to react.

In this case, the behavior is [::submit-gist](https://github.com/sreeharsha-m/lt-gist/blob/master/src/lt/plugins/gist.cljs#L22).  

It's a one line anonymous function that executes a process 'gist' on the cmd line passing the current file as args and stores the OUT in our [gist object](https://github.com/sreeharsha-m/lt-gist/blob/master/src/lt/plugins/gist.cljs#L35).

> An **object** doesn't do anything. It captures state so others can pass it around. Optionally, it lists behaviors. Like a class, but much better, because behaviors at runtime. It also has initializers, among other things.

Now that you understand how commands work, how they trigger reactions, you can run any Lighttable plugin, including Lightable itself, using your own keyboard shortcut in one line of code in `User.keymap`:

    "ctrl-shift-g" [:gist.submit] 

No big deal, you can add keybindings in many IDEs. However, Lighttable can modify its behavior, with a few lines of Clojurescript. Like good Lisp programs, it empowers you with new commands that let you read and write interesting things from any server.

### Modify and run a plugin from source

For plugin development, you must clone the plugin repo. If you already installed the plugin via Plugin manager as described above, uninstall the plugin.

How to clone the repo into your plugins folder:

  * On OS X: `~/Library/Application Support/LightTable/plugins/`
  * On Linux: `~/.config/LightTable/plugins/`
  * On Windows: `%APPDATALOCAL%/LightTable/plugins/`

As an example, clone [lt-gist](https://github.com/sreeharsha-m/lt-gist).

Open src `gist.cljs`.

Save `gist.cljs` OR Run `Editor: Build file or project`. 

You should see "Compiled plugin to ...gist_compiled.js" in the status bar.

`Ctrl-Space` and start typing `Plugins: Refresh plugin list`.

Detect your plugin.

Save `gist.behaviors` or run the command `App: Reload behaviors` to load/reload the plugin behaviors.

Open your sample clojure code.

`ctrl-shift-g`.

Great, a gist was just created and its url was copied onto the clipboard and status bar. Check it. Paste the url in the browser.

That's your gist.

Let's debug this plugin: uncomment the following code in gist.cljs.

    (.log js/console path)

`Cmd-Enter` to evaluate it. 

Lighttable asks for a client, select 'Light Table UI'. Save `gist.cljs` again.

`ctrl-shift-g`.

See the log printed right inside lighttable's console (`View Console`).

What we just did is not possible in any IDE, except for Emacs. 

IDEs like Sublime or Vim can be configured too, with a variety of plugins and options, but they can't modify their behavior or user interface at runtime. You need to change the source code. Also, their plugin portability is limited by their language (Python or Vim script), and simple things in one IDE may not be simple in another. 

For example,
 > You can't print a variable containing "hello world" and pipe its resulting String back into your IDE, on any part of your IDE, using nothing but your IDE, without modifying its source.

Why is this important? 

Lighttable plugins are written in a platform independent language, Clojurescript, and compiles to Javascript targeted for NodeJs. Modifying the behavior of an app and its life cycle, using commands from a native langauge understood across client and server, enables contextual, intelligent, and infinitely customizable systems.

In Lighttable, such extensions are possible with plugins.

For further insight, read the seminal essay by Chris Granger: [The IDE as Value](http://www.chris-granger.com/2013/01/24/the-ide-as-data/).

## Status

Working Draft.

I'm still learning.

## LICENSE

Attribution-NonCommercial 2.0 Generic. See [LICENSE](http://creativecommons.org/licenses/by-nc/2.0/).
