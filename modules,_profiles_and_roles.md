## Modules, Profiles and Roles

The "[Module, Profile, Role](https://puppetlabs.com/presentations/designing-puppet-rolesprofiles-pattern)"[3] pattern is the preferd way of combining simple modules to achieve a greater purpose.

A way to look at the pattern is:

* **Module**: is a single piece of technology. For example: apache or php.
* **Profile**: is a collection of technologies and common configuration. For example: Wordpress. A Wordpress profile includes, apache, php and the Wordpress framework. As well as a way to declare a new vhost in apache and configuring the Wordpress instance.
* **Role**: is a collection of profiles that configure a **host**. For example: We can use a Wordpress profile and a SendMail profile to configure **hotgod.com** and maybe **cutekitties.org**.





---

[2] Single responsability principle: http://www.objectmentor.com/resources/articles/srp.pdf

[3] Designing Puppet: Roles/Profiles Pattern: https://puppetlabs.com/presentations/designing-puppet-rolesprofiles-pattern
