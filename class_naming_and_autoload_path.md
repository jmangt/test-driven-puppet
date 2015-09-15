## Class Naming and Autoload Path

Puppet is picky when it comes to naming your files. If your want Puppet to play nice you need to name your files in classes in a specific way.

You can read about the subject in the "[Language: Namespaces and Autoloadin](https://docs.puppetlabs.com/puppet/latest/reference/lang_namespaces.html)g" section of the Puppetlabs website.

In a nutshell:

* The main class of the module must live in the `init.pp` file.
* Classes and Defines must have a file of their own.

```puppet
# mymodule/manifests/init.pp
class mymodule(){
  include mymodule::directories
}

# this class should be placed in the directories.pp file
class mymodule::directories{
...
}
```

* Internal Classes and Defines must be namespaced according to the modules name.