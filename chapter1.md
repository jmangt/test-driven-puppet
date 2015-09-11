# Setting up your development environment

Rule number 1 of the Puppet Club:

**We don't write or test code in our production environment.**


---

It's ok if you done so in the past. We all have. It just comes with the job. Having said that, you know is not cool, right?

The hardest part of writing and testing Puppet code is to be able to write and test Puppet code, is having and environment that truly resembles production.

You want your code to run. But if you are developing under a Mac system and your production server is running Centos 7, how can you be sure that what you wrote will actually work?

Most of the time we don't. 

So the solution is quite simple. Get a clone of your production environment and develop under it.

Here is a list of tools that will help us with that task.



