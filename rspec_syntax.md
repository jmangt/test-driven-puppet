### Rspec Syntax

Rspec uses a nice human readable syntax to describe your features. It follows commonly used convention for writing "user stories". 

The basic idea is that your declare A SUBJECT to describe. Then you present a CONTEXT where this SUBJECT is to be tested. Within the CONTEXT you DESCRIBE a feature of your SUBJECT. An finally you ASSERT if the FEATURE responded as expected.

Rspec expects your to place your files under a `specs` directory directly in your projects root. And each test file has to be named in the `*_spec.rb` form. 

An example for describing a DOG would look like this:

```ruby
# dogs/spec/dog_spec.rb
require 'spec_helper'

describe 'Dog' do
  context 'when sleeping' do
    describe '#bark' do
      it 'should not bark' do
        expect(Dog.bark).to == nil
      end
    end
  end
  
  context 'when awake' do
    describe 'bark' do
      it 'should be loud' do
        expect(Dog.bark).to == 'WOOF!'
      end
    end
  end
end
```

[The Rspec documentation](https://relishapp.com/rspec/) [4] site is the resource to use when it comes to syntax and features. So I will not go into to much detail into it. 

Take some time to go through the ["Getting Started](https://relishapp.com/rspec/docs/gettingstarted)" [5] section of the site if you want to go deep on how to use Rspec with a Ruby project.

### Assertions

An assertion or expectation is the code that tests your SUBJECT's behavior. In the Dog example the `it do ... end` blocks are the ones that contain the assertion.

In the first scenario the assertion expects that the Dog won't bark if it is asleep. If the Dog's bark would have been something different from `nil` Rspec would have marked the test as failed.

We will use the same logic to test our Puppet modules. But the expected behavior will be checking if the intended Puppet resources are being correctly declared in the compiled manifest.I will go a bit deeper into this in following sections.



---

[4] Relish Rspec: https://relishapp.com/rspec/

[5] Rspec Getting Started Guide: https://relishapp.com/rspec/docs/gettingstarted
