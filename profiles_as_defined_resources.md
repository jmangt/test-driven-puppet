## Profiles as Defined Resources

The previous examples were written with the assumption that your `profile` will only be used once per `role`.

In case your are in need to write a role that uses your `profile` several times with different values, you will get a "you can not define resource twice" error from Puppet.

To avoid this, the `profile` should be written as a custom resource.

Going back to our Wordpress example. I we where to do:

```puppet
# mysharesite/manifest/init.pp
class mysharedsite{
  class{'wordpress':
    name => 'hotdog.com',
  }
  
  class{'wordpress':
    name => 'blog.hotdog.com',
  }
}
```

This would fail with a `Error: Duplicate declaration: ... is already declared in file ...; cannot redeclare at ... on node ...` error.

If we want to avoid this our `wordpress` profile should be re writen as a custom resource. The concept behind what the profile does remains the same. We just need some syntax re adjustments.

A modified version of the `wordpress` module would move the host configuration to a different file. The `wordpress` class changes to becomes just a place holder for the namespace.

```puppet
# wordpress/manifests/init.pp
class wordpress (...) {
}

# wordpress/manifests/host.pp
define wordpress
```

