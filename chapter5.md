# Testing Puppet Modules

Here we are! 

Finally, the reason your started going through this book in the first place.

If you skipped any of the previous chapters, be aware that I will base this work flow on those tools, design patters and methodologies. If you stayed we me all the way: Thank you! You are a really cool and patient person.

I believe in learning by doing. As such, in the following sections I will put together a couple of simple modules and build them using a Test Driven approach. 

We'll start by writing a simple `users` module compatible with Ubuntu 14.04. In order to start with the right foot we will use the `puppet module generate` command and use the included `rspec` gem to test our clases. Then we will use the `beaker` gem to run acceptance tests by generating a virtual machine, applying our module an testing for expected outcome.

I will do my development from inside a Vagrant box running Puppet 4.2.2 and Ruby 2.2.1.

Ready? Here we go...


