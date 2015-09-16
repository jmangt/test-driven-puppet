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

The `metadata.json` file must be placed in the root of the project otherwise The Forge won't be able to find it.

This is an example of an `metadata.json`:

```json
{
  "name": "examplecorp-mymodule",
  "version": "0.0.1",
  "author": "Pat",
  "license": "Apache-2.0",
  "summary": "A module for a thing",
  "source": "https://github.com/examplecorp/examplecorp-mymodule",
  "project_page": "https://forge.puppetlabs.com/examplecorp/mymodule",
  "issues_url": "https://github.com/examplecorp/examplecorp-mymodule/issues",
  "tags": ["things", "stuff"],
  "operatingsystem_support": [
    {
    "operatingsystem":"RedHat",
    "operatingsystemrelease":[ "5.0", "6.0" ]
    },
    {
    "operatingsystem": "Ubuntu",
    "operatingsystemrelease": [ "12.04", "10.04" ]
    }
   ],
  "dependencies": [
    { "name": "puppetlabs/stdlib", "version_requirement": ">=3.2.0 <5.0.0" },
    { "name": "puppetlabs/firewall", "version_requirement": ">= 0.0.4" }
  ]
}
```

All the dependencies declared within the `metadata.json` are modules hosted in the Forge. If your dependencies are not publicly available then *you can not declare them here*.


---

[1] A repository of modules written by our community for Puppet Open Source and Puppet Enterprise IT automation software:  https://forge.puppetlabs.com/

[2] Publishing Modules on the Puppet Forge: https://docs.puppetlabs.com/puppet/latest/reference/modules_publishing.html

[3] Write a metadata.json File: https://docs.puppetlabs.com/puppet/latest/reference/modules_publishing.html#write-a-metadatajson-file
