# Test Driven Puppet

*This is a work in progress.*

## Dedication

To all my peeps at Xoom Coorporation who put up with my all my rants.

## Preface

### Why should your Test your Puppet modules?

**"Insanity is doing the same thing over and over again and expecting different results" -- Albert Einstein**.

**"Puppet code is software too. It deserves love an respect." -- JManGt**

If you've doing Puppet code for a while you probably are familiar with the phrase "Why did it do that?". Or maybe "The run failed but If when I ran it again it fixed itself". Or "Don't give it to the new guy He might brake it." 

You know the drill. 

In essence must of your Puppet code is unreliable. Or it is so brittle that just breathing on it might break it ( or running on the second Monday of the month ).

So what can you do to make sure that your Puppet code does what it meant to do? Test it.

"What do you mean with 'test it'?" you would ask. 

"Should I go into my production server and start changing code and applying it until I get the expected result? (once)".

"Or maybe I should have a staging server where I do all my testing. Then when I'm sure that I got it to run I'll just clone it to production."

Yes you could probably get by with a strategy like that. But do you want do?

In the following chapters I will present you with tools, strategies and patterns that will help with this task.

My goals for the following chapters are:
* Share the knowledge how to setup a tool chain that would allow you to write and test all your Puppet code. Either in isolation or in conjunction with other modules. 
* Present a pattern for structuring modules that encourages separation of concerns.
* Provide a way to manage dependencies between your private and public modules.
* And, to make a happier Puppeteer out of you.

Enjoy,

Javier Alvarez ([@JmanGT](http://twitter.com/jmangt))








