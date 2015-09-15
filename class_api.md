## Class API

A classes should be treated like an atomic unit. From the perspective of the module's user there should be **one and only one ** way to interact with it. And that is the class parameters.

**Your class parameters define your class API.
**

Deciding how to design your class API can be tricky, and it will require some trial an error to get it right.

My suggestion is to use the time tested `it's magical` approach.

This means that you should treat your class like it is magical. It should be able to read  your mind and meet your needs without you telling it anything. 

1. Name it. 
2. Run it. 
3. Did it do what you wanted? 
4. No? 
5. What did you miss? 
6. A parameter? Add it. 
7. Repeat

And so on and so forth.

***Providing a good API for your class might be the most important part of making your code testable.***

A solid API will allow you provide multiple test scenarios. Each scenario will be based on the combinations of your parameters.

```puppet
# manifests/init.pp
class helloworld(
  $salutation = $::helloworld::params::salutation,
  $who        = $::helloworld::params::who,
  $owner      = $::helloworld::params::owner,
) {
...
}
```

For example, class helloworld has tree parameters `salutation`, `who` and `owner`. This gives you at least eight scenarios, based on whether you pass or don't pass values to the class when your call it.

Where is two of them:

* what happens when i call the class without passing any parameters?
*what happens when i call the class passing all parameters?"

From there, and depending what your class does with it's parameters, you will find yourself with as many scenarios as parameter combinations you can produce.

A rule of thumb for selecting parameters is *"Pass as little parameters as you can to get the job done. Manage other options with sensible defaults."*.
