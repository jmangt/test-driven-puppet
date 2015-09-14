# Organizing Puppet modules

If you want to test your Puppet modules, you need to write your modules in a testable way.

In the case you are dealing with legacy code, I recommend that you set some time to refactor those modules. 

Use git and gitflow to allow you start a new `feature/refactor` branch. There you will be able to add new functionality to your module an ensure that your production branch is stable.

The folks at Puppetlabs have taken the time to write great documentation on how to design and structure your modules. Please take some time to go over the ["
Beginner's Guide to Modules](https://docs.puppetlabs.com/guides/module_guides/bgtm.html#beginner%27s-guide-to-modules)" section in the https://docs.puppetlabs.com website [1].

I would like to just highlight some design decisions that will impact in the way you test your modules.

## Modules

Write your modules following the 'Single responsabilty principle' (SRP) pattern [2].

Classes

Parameters

Autoload paths

Module, Profile, Role



---

[1] Beginner's Guide to Modules: https://docs.puppetlabs.com/guides/module_guides/bgtm.html#beginner%27s-guide-to-modules

[2] Single responsability principle: http://www.objectmentor.com/resources/articles/srp.pdf