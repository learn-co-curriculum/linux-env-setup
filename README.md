# Ubuntu Environment Setup

## Setting up your Ubuntu Developer Environment

In this readme we are going to go over the steps for setting up your development environment in Ubuntu. Specifically, we wrote these instructions with Ubuntu 15.10 in mind using the MATE Desktop Environment 1.10.2.

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
 - `curl -sL https://deb.nodesource.com/setup_5.x | sudo -E bash -`
 - `sudo apt-get install nodejs`
 - `sudo dpkg --add-architecture i386` This will install an architecture needed to install Team Viewer later.

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
 - `echo "[[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm" # Load RVM into a shell session *as a function*"`
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

One thing you'll see more of later in the course is npm, or [Node Package Manager](https://en.wikipedia.org/wiki/Npm_(software)). This is very similar to Ruby gems, but it's for JavaScript. We're also going to do a global install for an npm testing package Protractor with the following command:

 - `sudo npm install -g n`
 - `sudo npm install -g protractor`

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

### Team Viewer

You're going to want to download and install Team Viewer for Ubuntu [here](https://www.teamviewer.com/en/download/linux/). This is the tool we use at learn to screen share!

### Atom - Essential (or at least _some_ code editor)

Atom is github's hackable code editor. It's got tons of great tools for writting code. You can get it by following [these instructions](https://github.com/atom/atom#debian-linux-ubuntu)

### Synapse

This is just a quick launcher. It has a blue S logo at the top of your screen. Right click, select preferences and you can set up a shortcut for the Activate command. This just lets you pull up a bar to quickly find and run applications.

`sudo apt-get install synapse `

### ScudCloud - Slack for Linux

If you're not already familiar with Slack, it is a messaging app that is widely used in many tech companies and it is heavily used by the Learn community for students to chat and stay in touch with each other. If you're going through Learn, you should be on Slack.

```
sudo apt-add-repository -y ppa:rael-gc/scudcloud
echo ttf-mscorefonts-installer msttcorefonts/accepted-mscorefonts-eula select true | sudo debconf-set-selections
sudo apt-get update
sudo apt-get install scudcloud
```

For more information on ScudCloud, you can check out their github: https://github.com/raelgc/scudcloud

### Slack Beta

You can try the official beta client, but keep in mind that it is still in the works. There are binary packages for Ubuntu and Fedora that can be found here: https://slack.com/downloads

Download the 64-bit version for Ubuntu, `cd ~/Downloads` and run:

```
sudo dpkg -i slack-desktop-*.deb
```

### Parcellite

This app stores your clipboard history and makes it really easy to paste anything from your history. Take a look at the settings and you can customize your keybindings for it.

First, download [Parecellite](https://apps.ubuntu.com/cat/applications/precise/parcellite/) and then install xdotool with `sudo apt-get install xdotool`.

>Note: To set up one click paste from history, open up the Parecellite preferences. In the Behavior tab, select Auto Paste and Key and unselect Mouse. Then in the Hot Keys tab, just make sure you have something set for History Key Combination. Now when you hit those keys, you get your last X (X is number of items in history) copied items for easy pasting!
