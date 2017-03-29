- Start Date: 2016-07-24
- RFC PR:
- ember-cli Issue:

# Summary

This feature would allow blueprints and ember upgrades to interactively merge JSON files instead of wiping away existing files, or causing syntax errors due to manual text editing.

# Motivation

The current process for including things like `bower.json`, `package.json`, `.babelrc`, etc leads the user to have to manually read through diff logs and resolve change issues.
Most often, this occurs with the `ember-cli` app upgrade blueprint.
Especially for newer developers, it can be confusing why some of your addons get uninstalled in the process of upgrading.

# Detailed design

Using [three-way-merger](https://github.com/bantic/three-way-merger) you can actually do a compare between the currently installed version of ember-cli new output to the users local files to the ember-cli version that the use is upgrading to. By doing this it will preserve the users installed dependencies and removed dependencies, i.e. if they remove `ember-data` from the package.json it would not add it back in after doing the three way merge. 

Because of the way [three-way-merger](https://github.com/bantic/three-way-merger) generates it's output. You will be able to prompt the user in which dependencies will be added, changed, and removed. We can then use ui.prompt to allow the user to interactivly select which dependencies they want in their final package.json file.

# Drawbacks

* We might have to improve [three-way-merger](https://github.com/bantic/three-way-merger) to be able to have this also work for other json files e.x. bower.json

# Alternatives

Another way around this could be to use a descriptive broccoli style DSL to allow changes to common files such as `package.json` and `bower.json` (especially for upgrades).

# Unresolved questions

* How do we tell when files should use the traditional merge and diff vs JSON diff (file extension, registration in the blueprint, etc)?
* Should this be the default behavior during `ember init`
* Should we introduce a new command `ember upgrade:deps`
