## The users module

Create a new `users` module using the `puppet module generate myorg-users`.

We use the `myorg` prefix as a way to declare who's user module this is. This is required by the Forge in order to distinguish between two modules with the same name. For example: `bob-apache` and `mike-apache`.

Check your options by passing the `--help` flag.

```bash
uppet module generate --help

USAGE: puppet module generate [--skip-interview] <name>

Generates boilerplate for a new module by creating the directory
structure and files recommended for the Puppet community's best practices.

RETURNS: Array of Pathname objects representing paths of generated files.

OPTIONS:
  --render-as FORMAT             - The rendering format to use.
  --verbose                      - Whether to log verbosely.
  --debug                        - Whether to log debug information.
  --skip-interview               - Bypass the interactive metadata interview

See 'puppet man module' or 'man puppet-module' for full help.
```
