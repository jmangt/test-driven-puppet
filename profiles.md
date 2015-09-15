## Profiles

A **profile** is a collection of technologies, brought together for a specific purpose.

ere a are some examples:
* a Wordpress Site
* a Ruby on Rails Application
* a Node.js Application

A profile's responsibility is too coordinate multiple pieces of technology. Not by managing them, but by delegating that responsibility to their specific modules. 

Must of the time a profile will set up scaffolding for an application using that particular technology stack.

### A word of warning

The next examples of profiles are meant to be used once per role.** If your profile is used more than once per role Puppet will complain of not being able to re declare a resource**. If this is your use case you should explore the option of using custom resources with `define` instead on `class`.

Please read on the ["Defined resource types](https://docs.puppetlabs.com/puppet/latest/reference/lang_defined_types.html)" section of the Puppetlabs site. [1]



---
[1] Language: Defined Resource Types: https://docs.puppetlabs.com/puppet/latest/reference/lang_defined_types.html#language:-defined-resource-types

