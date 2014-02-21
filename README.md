# LightTable Plugin guide

A guide to building plugins in LightTable.

## Abstract

Gamers call it CES. Lispers call it REPL. LightTable can bring them both to your browser, desktop, and beyond.

## Introduction

Lightable plugin development in a nutshell:

    I command LT to do something
    Command executes and raises Triggers
    Triggers are captured by one or many Behaviors
    They listen to them from a pool
    Once they capture a trigger
    They respond with Reaction    
    a Reaction is just an anonymous fuction
    that bares no access to the outside
    But enclosed in THIS
    THIS is Lighttable
    Sharing I/O
    Realtime
    
This introductory chapter shows you first: how to download, run, and modify a light plugin from source. By doing this, we can understand how things are put together. 

Once we figure that out we can create new plugins.

### Pre-Requisite

Make sure LT_HOME is setup on your ENV.

Instead of clicking the icon, start Lighttable via command line:

    light

This way, Lighttable can access your ENV, enabling it to fork a process and run a unix command and pipe its output into Lightable's reactive surface. 

We'll see what this means, shortly.

For this guide, we'll study [lt-gist](https://github.com/sreeharsha-m/lt-gist), a plugin we wrote as an exercise. To run the plugin you need to run on the cmd line:  `gem install gist`.

**lt-gist** is a stupid plugin that gists your InstalRepl (or file), anonymously.

### Install a plugin

`Ctrl-Space` and start typing: `Show plugin Manager`.

> `Ctrl-Space` opens the command navbar, showing available commands. You can start typing in the command navbar to filter your list

Select `Available` and browse for `Gist`. 

`Install`.

Now, create a new file and save it as .clj. Write _hello world_ or something simple. Save.

`Ctrl-Space` and start typing ... `Gist`, select it.

Open a new tab and hit paste (from clipboard).

Congrats, you just gisted your code anonymously, from Lighttable!

### Setup keybindings

**Short answer**: `Ctrl-Space` and start typing `Settings: User keymap`, select it. In the opened tab, under the `:editor` map of existing keys shown in `{}`, paste the following key value pair: 

    "ctrl-shift-g" [:gist.submit]
    
### Understanding Commands, Behaviors, Triggers, and Objects

The first step of customizing your plugin is setting up commands. 

Read the plugin's source code and grep for `(cmd/command`. It's just a map with few keys. Look for the value of `:command`. In `lt-gist` it's [:gist.submit](https://github.com/sreeharsha-m/lt-gist/blob/master/src/lt/plugins/gist.cljs#L41).  

> `:command` points to another key, an element in a set of Triggers, composed in a Behavior ready to react as a Reaction.

In this case, it's [::submit-gist](https://github.com/sreeharsha-m/lt-gist/blob/master/src/lt/plugins/gist.cljs#L22).  

It's a one line anonymous function that executes a process 'gist' on the cmd line passing the current file as args and stores the OUT in our [gist object](https://github.com/sreeharsha-m/lt-gist/blob/master/src/lt/plugins/gist.cljs#L35).

> An **object** can, optionally, list its behaviors. Like a class, but every bit better, because it composes _its own_ methods at runtime

Now that you understand how commands work, how they trigger reactions, you can run any existing Lighttable or plugins commands using your own keyboard shortcut, by writing just one line of code in `User.keymap`:

    "ctrl-shift-g" [:gist.submit] 

No big deal, you can add keybindings in many IDEs. 

Like Emacs, Lighttable can modify its behavior, with just a few lines of Clojurescript.

### Run and modify a plugin from source

For plugin development, you must clone its repo. If you already installed the plugin via Plugin manager as described above, uninstall it.

How to clone the repo into your plugins folder:

  * On OS X: `~/Library/Application Support/LightTable/plugins/`
  * On Linux: `~/.config/LightTable/plugins/`
  * On Windows: `%APPDATALOCAL%/LightTable/plugins/`

Clone [lt-gist](https://github.com/sreeharsha-m/lt-gist).

Open `gist.cljs`.

Save `gist.cljs` OR Run the command `Editor: Build file or project`. 

You should see "Compiled plugin to ...gist_compiled.js" in the status bar.

Ctrl-Space `Plugins: Refresh plugin list` to detect the plugin.

Save `gist.behaviors` or run the command `App: Reload behaviors` to load/reload the plugin behaviors.

Open your sample clojure code.

`ctrl-shift-g`.

Great, gist is created and its url is copied onto the clipboard. Paste the url in the browser.

Let's push this further, let's debug this plugin: 

Uncomment the following code in gist.cljs.

    ;;(.log js/console path)

`Cmd-Enter` to evaluate it. Lighttable asks for a client, select 'Light Table UI'. Save `gist.cljs` again.

`ctrl-shift-g`.

See the log printed right inside lighttable's console (`View Console`).

What we just did is not possible in any IDE, except for Emacs. 

IDEs like Sublime or Vim let you configure them with a variety of plugins and options, but won't modify or distribute their behavior at runtime. Their plugins' portability is limited by their language (python or emacs-lisp), and simple things in one IDE may not be simple in another. What's more, unlike others, Lighttable plugins are written in a platform independent language, clojurescript, that compiles to javascript on browser or node. 

Modifying the excution in any step of a command's life cycle, using a single langauge across the wires, is the kind of stuff that empowers lispers. 

Now it's all yours.

For further insight, re-read Granger's seminal essay: [The IDE as Value](http://www.chris-granger.com/2013/01/24/the-ide-as-data/).

## Other chapters

TODO. 

## Status

Working Draft.

For other chapters, see `drafts` github branch. 

Pull requests or suggestions are welcome.

## LICENSE

Attribution-NonCommercial 2.0 Generic. See [LICENSE](http://creativecommons.org/licenses/by-nc/2.0/).