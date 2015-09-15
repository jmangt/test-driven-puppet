## Profiles as Defined Resources

The previous examples were written with the assumption that your `profile` will only be used once per `role`.

In case your are in need to write a role that uses your `profile` several times with different values, you will get a "you can not define resource twice" error from Puppet.

To avoid this, the `profile` should be written as a custom resource.