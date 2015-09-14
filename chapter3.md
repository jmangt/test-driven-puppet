# Organizing Puppet modules

If you want to test your Puppet modules, you need to write your modules in a testable way.

In the case you are dealing with legacy code, I recommend that you set some time to refactor those modules. 

Use git and gitflow to allow you start a new `feature/refactor` branch. There you will be able to add new functionality to your module an ensure that your production branch is stable.

The folks at Puppetlabs have taken the time to write great documentation on how to design and structure your modules. Please take some time to go over the ["
Beginner's Guide to Modules](https://docs.puppetlabs.com/guides/module_guides/bgtm.html#beginner%27s-guide-to-modules)" section in the https://docs.puppetlabs.com website [1].

I would like to just highlight some design decisions that will impact in the way you test your modules.

## Classes

A class is a collection of resources that should be considered *independent* and *encapsulated*.

This means that your `class` should be able to fend for itself.

Your class must not rely on any other resources to be present in your system. unless it checks for them by itself or delegates that task to another class.

And anything that your `class` needs should come from the class's `parameters`.



## Modules

*My definition* of **module** is: "*A collection of Puppet resources that administer one and only one piece of technology*".

That means that:
*  an apache **module** manages Apache
*  an nginx **module** manages Nginx
*  an ntp **module** manages Ntp

If you are not sure where to draw a line in you module's design, read a bit on the 'Single responsabilty principle' (SRP) pattern [2]. This design pattern is use widely in Object Oriented Programming (OOP) and can be applied to your Puppet code as well.



A module 

Classes

Parameters

Autoload paths

Module, Profile, Role



---

[1] Beginner's Guide to Modules: https://docs.puppetlabs.com/guides/module_guides/bgtm.html#beginner%27s-guide-to-modules

[2] Single responsability principle: http://www.objectmentor.com/resources/articles/srp.pdf