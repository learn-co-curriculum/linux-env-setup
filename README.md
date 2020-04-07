# Ubuntu Environment Setup

## Setting up your Ubuntu Developer Environment

In this readme we are going to go over the steps for setting up your development environment in Ubuntu. Specifically, we wrote these instructions with Ubuntu 18.04 in mind using the MATE Desktop Environment 1.20.1.

First off: **you need to be a user with `sudo` access to perform this setup**. You can check to see if you have sudo access by typing `sudo echo ok` in your terminal. If you get back `ok`, then you have the correct access.

We also want to make sure that your terminal is set to be a login shell. Setting your terminal to a login shell ensures that our .bash_profile will be run. In Ubuntu, open up your terminal, select Edit > Profile Preferences. Then on the Command tab, check off 'Run command as login shell'.

## Set up group

He we are setting up a group with the name "npm". This is so that we can set reasonable permissions on packages that get installed via npm (more on permissions and npm later). These permissions ensure that we can globally install npm packages.

 - `sudo groupadd npm`
 - `sudo usermod -a -G npm,staff $USER`

## Make sure everything is up to date

Before we start installing all of our developer tools, we want to make sure that everything is up to date. For this, we're going to use the `apt-get` Linux package manager to update everything using the following commands:

 - `sudo apt-get update`
 - `sudo apt-get upgrade`
 - `sudo apt-get dist-upgrade` (this one is optional)

## Installing Dev tools

### The essentials

Now we can install some essential dev tools (postgres, node...) using curl:

 - `sudo apt-get -y install curl postgresql libpq-dev default-jre build-essential phantomjs`
 - `curl -sL https://deb.nodesource.com/setup_11.x | sudo -E bash -`
 - `sudo apt-get install nodejs`

### The non-essentials:

 - `sudo apt-get -y install ack-grep vim libgnome2-bin`

You can also install a few non-essential tools (ack-grep, vim, and libgnome) if you like:

 - [ack-grep](http://beyondgrep.com/) - A more advanced tool to take the place of the `grep` (search) command in the terminal
 - [vim](https://en.wikipedia.org/wiki/Vim_(text_editor)) - A terminal based text editor
 - [libgnome2-bin](https://developer.gnome.org/about/about.html.en) - Provides documentation for the GNOME desktop environment and acts as a handy open tool.

## Setting file permissions

To be able to properly use some of these tools, we need to manually set some of the permissions and owners of some of the recently installed files:

 - `sudo chown root:staff /usr/bin`
 - `sudo chmod 0775 /usr/bin`
 - `sudo chown -R root:npm /usr/lib/node_modules`
 - `sudo chmod 0775 /usr/lib/node_modules`

If you're curious, you can read more about `chmod` (setting permissions) and `chown` (setting the owner) here: http://www.unixtutorial.org/2014/07/difference-between-chmod-and-chown/
Take a look at [this page](http://www.perlfect.com/articles/chmod.shtml) if you want to understand the permission values (0755, 0600, ...)

## Set up .netrc file for the learn gem

The learn gem needs this netrc file to work. The `.netrc` file is a standard location to store login/token info, so it is we store information needed for Learn.

 - `touch ~/.netrc && chmod 0600 ~/.netrc`

## Installing RVM

[RVM](https://en.wikipedia.org/wiki/Ruby_Version_Manager) is a great tool that lets you run different versions of Ruby on your computer. This is really useful because if you know one project your working on works with Ruby version 2.3.1 and another needs 2.6.1, you can easily switch between the two versions when you switch between projects. You can install it and set it up with the following commands:

 - `gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3`
 - `\curl -sSL https://get.rvm.io | bash`
 - `echo "[[ -s "$HOME/.rvm/scripts/rvm" ]]" && source "$HOME/.rvm/scripts/rvm" # Load RVM into a shell session *as a function*"`
 - `rvm install 2.6.1`
 - `rvm use 2.6.1 --default`

You can easily install different Ruby verions with `rvm instal <version number>`, switch between versions with `rvm use <version number>` and check to see which you've already installed with `rvm list`. You can always check what version your current terminal window is with `ruby -v`.

## Setting up (and getting) Ruby gems

If your familiar with any other program language, [Ruby gems](https://en.wikipedia.org/wiki/RubyGems) are like libraries. If you're not familiar with programing libraries, it's basically isolated chunks of code that you can easily add to your project. For example, the `learn-co` gem allows you to easily interface with Learn from your command line (opening and submitting labs among many other things).

To set up your gems and install some necessary ones, run the following commands:

 - `echo "gem: --no-ri --no-rdoc" > $HOME/.gemrc`
 - `gem update --system`
 - `gem install learn-co`
 - `gem install phantomjs`
 - `gem install pg`
 - `gem install sqlite3`
 - `gem install bundler`
 - `gem install rails`

## Installing a Node Pacakge

One thing you'll see more of later in the course is npm, or [Node Package Manager](https://en.wikipedia.org/wiki/Npm_(software)). This is very similar to Ruby gems, but it's for JavaScript.

 - `sudo npm install -g n`

## Setting up your computer up with Github

All of this courses content is stored on Github so you're going to have to do a lot of cloning (copying files from your github account to your computer) and pushing (taking files you've saved on your computer, and updating your github files with them).

It would be a real pain if you have to type your password in every time you wanted to perform one of these actions. Luckily you don't have to with an SSH key. You can set this up by following the following steps:

 - `ssh-keygen` (ONLY DO THIS IF YOU DON'T ALREADY HAVE A GENERATED SSH KEY: just press return for everything, and don't enter a passphrase)
 - `cat ~/.ssh/id_rsa.pub`. This will disply the output of your SSH key to your terminal. You can then copy that, and add as ssh key on github by following the [instructions posted on github](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/). Note: The first step in these instructions is _the same_ as doing the `cat ~/.ssh/id_rsa.pub` command and manually copying the text.
 - You're also going to want to let the git that is running on your machine to know you you are. You can set this up by running: `git config --global user.email "you@example.com"` and `git config --global user.name "Your Name"`

## Configure the Learn-Co gem

Now that you have most of your tools installed, you're going to want to configure your learn-co gem. As previously mentioned, this gem lets you run tests, open labs, submit them, and save them to finish later. You can configure the gem to your account by using the following command:

 - `learn whoami`
 - Enter OAuth token when asked
   - If you have connected your Github account to your Learn account, navigate to learn.co/your_github_username. The OAuth token is at the bottom of the page.
    - If you have not connected your Github account: Go to [your profile](https://learn.co/account/profile) > Learn Settings > Public Profile. Click on the link under **Username**. The OAuth token is at the bottom of the page.

## Optional Dotfiles

These dotfiles do a variety of different things and I highly recomend you download them.

 - `curl "https://raw.githubusercontent.com/flatiron-school/dotfiles/master/irbrc" -o "$HOME/.irbrc"` - This file gives you some nice formatting for when you're in IRB (IRB lets you write ruby code in your terminal)
 - `curl "https://raw.githubusercontent.com/flatiron-school/dotfiles/master/ubuntu-gitignore" -o "$HOME/.gitignore"` - Global .gitignore rules. When you add a .gitignore file to a project, it let's you specify certain files that you DO NOT want pushed up to github (like API keys...)
 - `curl "https://raw.githubusercontent.com/flatiron-school/dotfiles/master/linux_bash_profile" -o "$HOME/.bash_profile"` - Your bash profile loads up every time you open a terminal window. The Learn bash_profile is designed to load up a bunch of shortcuts for you as well as make sure that RVM loads up every time you open the terminal. I recommend you take a look at this file and even see if there are any shortcuts of your own that you'd like to add! Note: this will overwrite existing bash profile, so back up if you want to.
 - `curl "https://raw.githubusercontent.com/flatiron-school/dotfiles/master/linux_gitconfig" -o "$HOME/.gitconfig"` then `nano $HOME/.gitconfig` and edit what needs to be edited (github username and github email in a few places)

## Download Chrome and some other useful apps

### Chrome - Essential

The Google Chrome browser is the required browser for Learn. You can download and install it by going to www.google.com/chrome.

### Zoom Client

You're going to want to download and install Zoom for Ubuntu [here](https://zoom.us/download?os=linux). This is the tool we use at learn to screen share! Download the .deb file, then right click and open with the GDebi Package Installer.

### Atom - Essential (or at least _some_ code editor)

Atom is github's hackable code editor. It's got tons of great tools for writting code. You can get it by following [these instructions](https://github.com/atom/atom#debian-linux-ubuntu)

### Slack for Linux

If you're not already familiar with Slack, it is a messaging app that is widely used in many tech companies and it is heavily used by the Learn community for students to chat and stay in touch with each other. If you're going through Learn, you should be on Slack.

`sudo apt-get install slack-desktop `
