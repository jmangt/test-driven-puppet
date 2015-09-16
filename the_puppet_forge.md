## The Puppet Forge

The "[Forge](https://forge.puppetlabs.com/)"[1] is the place where all publicly available modules are hosted.

Publishing your own modules is "easy", as long as you follow their conventions. You can read on how to organize and publish your modules in the "[Publishing Modules on the Puppet Forge](https://docs.puppetlabs.com/puppet/latest/reference/modules_publishing.html)"[2] section of the Puppetlabs site.

### Module definition

The way that the Forge declares a module and it dependencies is the [`metadata.json](https://docs.puppetlabs.com/puppet/latest/reference/modules_publishing.html#write-a-metadatajson-file)`[3] file. 

This `json` file holds important information on:
* What is the name of the module
* It's version
* Where the module is hosted
* The operating systems your module supports
* And, the module's dependencies





---

[1] A repository of modules written by our community for Puppet Open Source and Puppet Enterprise IT automation software:  https://forge.puppetlabs.com/

[2] Publishing Modules on the Puppet Forge: https://docs.puppetlabs.com/puppet/latest/reference/modules_publishing.html

[3] Write a metadata.json File: https://docs.puppetlabs.com/puppet/latest/reference/modules_publishing.html#write-a-metadatajson-file
