# Learn Enough Dev Environment to Be Dangerous


## Development environment setup
\label{sec:jekyll-intro}

You will be using a framework called Jekyll to handle the processing and compiling of your pages---bringing multiple pieces of code together to build a complete site out of the parts.

![Not Jekyll and Hyde...rather Jekyll the static site generator!\label{fig:jekyll}](images/figures/jekyll.jpg)

Jekyll was created using the programming language Ruby, and is what is known as a Ruby gem. You can think of Ruby gems (or 'gems') as plug-ins that extend the basic Ruby application that comes pre-installed now with macOS\@. Each gem also comes with packaged instructions that tell your computer how to install other third party gems that are required as dependencies to make the plug-in work.

+++add material on C9?

Developers like systems like Jekyll because they remove the need to repeat things and because they allow us to manage the content for websites in a way that resembles writing code. Specifically, Jekyll is great because by using it:

- Can write content in markdown in your text-editor of choice
- Write and preview your content on your site locally in your dev environment
- Publish changes via git
- Your site is on a static web-server and pretty immune to traffic
- Host your site freely on GitHub Pages
- No database management

Gems are designed to be installed and work with whatever version of Ruby you have locally. If you were to not follow this guide and just try and install Jekyll with no other setup, you would install the gem and its dependencies in a way that directly ties them to the macOS system version of Ruby. That means that if you ever need to work on a project that requires a different version of Ruby, or if Apple changes the system Ruby, you will have to uninstall the gems that you installed, install a Ruby version manager, and then reinstall the gems.

Or more than likely you'd eventually have waste a significant amount of time trying to decipher terminal error messages that can be less than helpful. By properly setting up a Ruby version manager now you ensure that the package of gems that you install will be safely tied to a new copy of Ruby that you control, and it will protect the gems from possible conflicts in the future.

### Jekyll install
\label{sec:jekyll-install}

We are going to go through some of these steps pretty quickly as some steps are things that you done in previous tutorials, like Learn Enough Command Line and Learn Enough Text Editor, and others we'll move quickly past because getting deep into the how and why of the inner workings of Jekyll just isn't important at this point in your progress as a developer. This isn't Learn Enough Jekyll after all!

### Installing Xcode command line tools
\label{sec:shiny_xcode}

Xcode is a large suite of development tools and code libraries created by Apple, and it is a requirement for doing the kind of development covered by this tutorial. Thankfully, Apple has recently made Xcode incredibly quick and easy to install---it used to require a 4+ GB download of installation source files.

To install Xcode, open up your terminal and paste the following command in

\begin{codelisting}
\label{code:xcode-install}
\codecaption{Installing Xcode command line tools.}
```console
$ xcode-select --install
```
\end{codelisting}

You'll be prompted by macOS to confirm that you want to install Xcode, do that and that's it!

### Installing the Homebrew package manager
\label{sec:homebrew}

Homebrew is a command line based package manager for macOS\@. If the phrase "package manager" isn't familiar to you, you can think of it as an application that works like an App Store---only it is filled with free open-source software.

If by chance you have played around with Linux in the past then you might have used a package manager in the past to install applications and utilities. Even though macOS is built on a similar foundation as Linux, Apple decided not to include a built in package manager to let you easily install software. Homebrew is one of many managers that is available to the open-source community, but over time it has become one of the most popular options in the Ruby development world.

Installation of Homebrew is simple. First you are going to need to run a command from the terminal that is going to connect to the Homebrew repository, download it, and install it:

\begin{codelisting}
\label{code:homebrew-install}
\codecaption{Installing the Homebrew package manager.}
```console
$ /usr/bin/ruby -e \
> "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
\end{codelisting}

Hit enter when prompted to start the installation, and after everything finishes downloading and installing, it will suggest that you run this command to finish the installation. Do it!

```console
$ brew doctor
```
The `brew doctor` command starts a process that ensures that all of the directories and permissions needed by Homebrew to manage local files are corrected set up. If you have any problems at this point, you'll need to refer to the [Homebrew troubleshooting wiki](https://github.com/Homebrew/homebrew/wiki/troubleshooting) (you really shouldn't though unless you've been making changes to random system folders and permissions).

### Installing rbenv, a Ruby environment manager
\label{sec:rbenv}

Now we are starting to get to the core of the development environment! Rbenv is a utility that will run on your computer to manage any versions of Ruby you install and it will amke sure that the gems (plug-ins) you install are placed in the right spot for Ruby to find. The rbenv system is modular and allows you to specify a different version of Ruby (and the associated gems) for different project repositories.

The full level of functionality isn't really needed for this project, but if you continue to do web development you will find that you need to lock certain projects to certain versions of Ruby because of dependencies that will only work on a specific version of Ruby. That could cause you to be unable to use your local development environment without going through some annoying updating and reconfiguring of individual applications.

To get started, from the terminal run:

```console
$ brew install rbenv ruby-build
```

When the download and installation finishes, run the following at the command line to get rbenv up and running:

```console
$ rbenv init
```
Now, if you are like me, you aren't going to want to have to think about starting up a management utility like rbenv every time that you open up your terminal. To set your system to always be ready for development, you are going to need to add a command to your `bash_profile`. If you completed the Learn Enough Text Editor tutorial then you are already familiar with editing the `bash_profile`, if you didn't do that tutorial and / or don't know if you have a `bash_profile` created, run this command in your terminal:

```console
$ touch ~/.bash_profile
```

Right now we are only going to add a single line to it, if your computer already has a *bash_profile* from previous work, you can just open it up and check to see if the `eval "$(rbenv init -)"` line is already in it. Otherwise, to both create the file and / or add the command to start rbenv, run this in the terminal:

```console
$ echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
```

If you want to make sure that everything was added correctly, you can run this in the terminal:

```console
$ cat ~/.bash_profile
```

That will spit out the contents for the `bash_profile` into the terminal window, and if you see `eval "$(rbenv init -)"` everything is correct. The last thing that you need to do is to let the current terminal window that you are working in know that there is a new `bash_profile` with configurations to load.

```console
$ source ~/.bash_profile
```
You now have a Ruby environment manager that is ready to handle custom versions of Ruby and keep track of any gems that you install for each version.

### Installing a new Ruby version
\label{sec:install_ruby}

Now that your environment manager is set up, let's give it a non-system version of Ruby to manage. The installation process is handled entirely by rbenv, so all you have to do is instruct it as to which version you'd like on your system by passing along the exact Ruby version and patch version number like this:

\begin{codelisting}
\label{code:ruby-nstall}
\codecaption{Installing a fresh copy of Ruby.}
```console
$ rbenv install 2.1.3
```
\end{codelisting}

You will see rbenv start the download process and install any dependencies that are needed for that specific verion of Ruby. When the installation finishes, run:

```console
$ rbenv rehash
```
The `rehash` command lets rbenv know that there is a new version of Ruby on the system that it needs to make available for you to use.

For this guide, we are also going to make the 2.1.3 version the global default so that you won't have to worry about specifying the Ruby version when you start your project. To make the Ruby version you just installed into the default, run this in your terminal window:

```console
$ rbenv global 2.1.3
```

### Installing the Github-pages gem
\label{sec:gem}

The last piece of your dev environment setup is to install the actual framework gems that will allow you to start developing your application.

```console
$ gem install github-pages --no-ri --no-rdoc
```

You'll notice that the installation command has a funny ending. That `--no-ri --no-rdoc` tells the Ruby installer to ignore downloading the gem documentation. If you are interested in the details of how the gem works, feel free to not include the flags, but those documents do tend to increase the download's size and installation time (and you'll probably never read them).

The *github-pages* gem is actually a big package of a bunch of gems that includes Jekyll, plus all of its dependencies, and it is created by GitHub to make using Github Pages standardized and in general a whole lot easier.

You will see the terminal display the download and installation process for all of the gems and dependencies. When the installation has finished, you will need to let the terminal know that there are new commands available to use by resourcing your `bash_profile` using the command you used before:

```console
$ source ~/.bash_profile
```