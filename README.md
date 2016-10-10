Slate
========

Getting Started with Slate
------------------------------

### Prerequisites

You're going to need:

 - **Ruby, version 2.1.0 or newer**
 - **Bundler** — If Ruby is already installed, but the `bundle` command doesn't work, just run `gem install bundler` in a terminal.

### Getting Set Up

 1. Install all dependencies: `bundle install`
 2. Start the test server: `bundle exec middleman server`

> Note: If you get an error installing the native extensions for the `eventmachine` gem, try `gem install eventmachine -v '1.0.8' -- --with-cppflags=-I/usr/local/opt/openssl/include`.

You can now see the docs at <http://localhost:4567>. And as you edit `source/index.md`, your server should automatically update! Whoa! That was fast!

Now that Slate is all set up your machine, you'll probably want to learn more about [editing Slate markdown](https://github.com/tripit/slate/wiki/Markdown-Syntax), or [how to publish your docs](https://github.com/tripit/slate/wiki/Deploying-Slate).
