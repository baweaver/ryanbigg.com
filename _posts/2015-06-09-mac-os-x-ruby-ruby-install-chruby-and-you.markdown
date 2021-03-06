---
wordpress_id: RB-363
layout: post
title: Mac OS X, Ruby, ruby-install, chruby and You
ruby_version: 2.4.1
rails_version: 5.1.2
---

**Last updated: July 13th, 2016**

<p>
  <strong>This beginner's guide will set up with Ruby {{page.ruby_version}}, chruby, ruby-install and Rails {{page.rails_version}} and is specifically written for a <em>development</em> environment on Mac OS X, but will probably work on many other operating systems with slight modifications.</strong>
</p>

<p>This guide is <em>almost</em> a copy of my <a href='http://ryanbigg.com/2014/10/ubuntu-ruby-ruby-install-chruby-and-you/'>Ubuntu, Ruby, ruby-install, chruby, Rails and You</a> guide, but this one has instructions for Macs.</p>

This guide will cover installing a couple of things:

* [**ruby-install**](https://github.com/postmodern/ruby-install): a very lightweight way to install multiple Rubies on the same box.
* [**chruby**](https://github.com/postmodern/chruby): a way to easily switch between those Ruby installs
* **Ruby {{page.ruby_version}}**: at the time of writing the newest current stable release of Ruby.
* **Bundler**: a package dependency manager used in the Ruby community
* **Rails {{page.rails_version}}**: at the time of writing the newest current stable release of Rails.

By the end of this guide, you will have these things installed and have some very, very easy ways to manage gem dependencies for your different applications / libraries, as well as having multiple Ruby versions installed and usable all at once.

We assume you have `sudo` access to your machine, and that you have an understanding of the basic concepts of Ruby, such as "What is RubyGems?" and more importantly "How do I turn this computer-thing on?". This knowledge can be garnered by reading the first chapter of [any Ruby book](https://manning.com/black2).

If you're looking for a good Rails book, I wrote one called [Rails 4 in Action](http://manning.com/bigg2).

### Housekeeping

The first thing we're going to need to install is XCode which you can get from the Mac App Store. We'll use XCode to install the Command Line Tools which install some libraries that Ruby will use to compile itself.

    xcode-select --install

First of all, we're going to need to install some package management script so that we can install packages such as Git, MySQL and other things exceptionally easy. The best package management system on Mac OS X for this is [homebrew](https://brew.sh). We can install this by using this command:

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

Next, we'll install `chruby` and `ruby-install`.

    brew install chruby ruby-install

### ruby-install

First we fetch the ruby-install file, extract it into a directory, then make it. You can verify that these steps have worked by running the following command:

```
ruby-install -V
```

If you see this, then you've successfully installed ruby-install:

```
ruby-install: 0.6.1
```

### Ruby

Our next step is to install Ruby itself, which we can do with this command:

```
ruby-install --latest ruby
```

This command will take a couple of minutes, so grab your $DRINKOFCHOICE and go outside or something. Once it's done, we'll have Ruby {{page.ruby_version}} installed.

Now we'll need to load chruby automatically, which we can do by adding these lines to `~/.bash_profile` (or `~/.zshrc` if you're using ZSH):

```
source /usr/local/opt/chruby/share/chruby/chruby.sh
source /usr/local/opt/chruby/share/chruby/auto.sh
```

In order for this to take effect, we'll need to source that file:

```
. ~/.bash_profile
# or (if you're using ZSH)
. ~/.zshrc
```

Alternatively, opening a new terminal tab/window will do the same thing.

To verify that chruby is installed and has detected our Ruby installation, run `chruby`. If you see this, then it's working:

```
ruby-{{page.ruby_version}}
```

Now we need to make that Ruby the default Ruby for our system, which we can do by creating a new file called `~/.ruby-version` with this content:

```
ruby-{{page.ruby_version}}
```

This file tells `chruby` which Ruby we want to use by default. To change the ruby version that we're using, we can run `chruby ruby-{{page.ruby_version}}` for example -- assuming that we have Ruby {{page.ruby_version}} installed first!

Did this work? Let's find out by running `ruby -v`:

```
ruby 2.4.1p111 (2017-03-22 revision 58053) [x86_64-darwin16]
```

### Rails

Now that we have a version of Ruby installed, we can install Rails. Because our Ruby is installed to our home directory, we don't need to use that nasty `sudo` to install things; we've got write-access! To install the Rails gem we'll run this command:

    gem install rails -v {{page.rails_version}} --no-document

This will install the `rails` gem and the multitude of gems that it and its dependencies depend on, including Bundler.

### MySQL

Before you can use MySQL, you'll need to install it with Homebrew:

    brew install mysql

After this, `gem install mysql` should succeed.

### PostgreSQL

Before you can use PostgreSQL, you'll need to install it with Homebrew:

    brew install postgresql

After this, `gem install pg` should succeed.

### Fin

And that's it! Now you've got a Ruby environment you can use to write your (first?) Rails application in with such minimal effort. A good read after this would be the <a href='http://guides.rubyonrails.org'>official guides for Ruby on Rails</a>.

The combination of chruby and ruby-install is such a powerful tool and comes in handy for day-to-day Ruby development. Use it, and not the packages from apt to live a life of development luxury.
