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

This would fail with a `Error: Duplicate declaration: ... is already declared in file ...; cannot re declare at ... on node ...` error.

If we want to avoid this, our `wordpress` profile should be re written as a custom resource. The concept behind what the profile does remains the same. We just need some syntax re adjustments.

A modified version of the `wordpress` module would move the host configuration to a different file. The `wordpress` class changes to becomes just a place holder for the namespace. And any client modules will change to use our new `wordpress::host` resource.

```puppet
# wordpress/manifests/init.pp
class wordpress {
}

# wordpress/manifests/install.pp
class wordpress::install {
  include apache
  include php
}

# wordpress/manifests/site.pp
define wordpress::host (
  site_name => 'default', 
){
  include wordpress::install
}

# mysharedsite/manifests/init.pp
class mysharedsite{
  wordpress::host{'hotdog.com':
  }
 
  wordpress::host{'blog.hotdog.com':
  }
}
```

