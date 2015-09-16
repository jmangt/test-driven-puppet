# Distributing Puppet modules

Puppet modules are rarely self contained. Usually your module will depend on other public or private modules. And this dependency chain has a big impact on how we test our code.

This is an easy rule of thumb for testing code: **"Test unstable code using stable dependencies"**.

I know it does not sound like a big deal for a seasoned developer as yourself. But you might find yourself in a situation where the module you are working on needs a new feature from a dependent module. If that is case, take the time and add the feature to the new module. Test it and generate a new release of the module before going back to the module you were originally working on.

Remember, that dependent module might be used by other modules besides yours. And making sure that you did not break anything before releasing is just good manners.

## The Puppet Forge

The "[Forge](https://forge.puppetlabs.com/)"[1] is the place where all publicly available modules are hosted.

Publishing your own modules is "easy", as long as you follow their conventions. You can read on how to organize and publish your modules in the "[Publishing Modules on the Puppet Forge](https://docs.puppetlabs.com/puppet/latest/reference/modules_publishing.html)"[2] section of the Puppetlabs site.



---

[1] A repository of modules written by our community for Puppet Open Source and Puppet Enterprise IT automation software:  https://forge.puppetlabs.com/

[2] Publishing Modules on the Puppet Forge :https://docs.puppetlabs.com/puppet/latest/reference/modules_publishing.html