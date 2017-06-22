# posix-guidelines
Guidelines for writing your shell scripts in the POSIX standard

`Version: 0.1.0`
<sub><sup>*[semver.org](http://semver.org/)*</sup></sub>

## Indicate inside your shell scripts that you are using posix-guidelines
Copy and paste in the header of each script:
```sh
# This POSIX sh is following the "posix-guidelines" version="0.1.0" {
#   <https://github.com/lbezerril/posix-guidelines/blob/master/README.md>
# }
```

### Structure
* The first line of your script should be `#!/bin/sh` or `#!/usr/bin/env sh` (Of course, you can include arguments)
* Do not use `;` at the end of the line
* Two spaces should be used as the unit of indentation.
* Avoid long lines. Set a maximum size of columns, and if it exceeds, use `\` to continue on the next line

### Naming convention
* For functions, use underscores with lower case and an underscore prefix, as in `_foo_bar()`
* For functions that will be sourced¹, use underscores with lower case and a double underscore prefix, as in `__foo_bar()`
* For variables, use underscores with lower case, as in `variable_name="$1"`
* For constants, use underscores with upper case, as in `CONSTANT_NAME="value"`

### Variables and Constants
* Local variables should be declared with `local`, including loops, as in:
```sh
local dir
for dir in /bin /sbin /usr; do
  echo $dir
done
```
* Constants should be declared at the beginning of a script or function.

### Comments
* Functions should contain clear comments about your goal. A description of the expected arguments, as well as the stdin if applicable, should also be commented on. Examples:
```sh
# Return a list of registered users in the database
_get_users() {
  echo "implementation ..."
}

# Register a new user
_create_user() {
  # $1: The username
  # $2: The user password
  # $@: Comma-separated list of phone numbers

  echo "implementation ..."
}

# Removes a list of users
_drop_users() {
  # stdin: Comma-separated list of usernames

  echo "implementation ..."
}
```
* If you have a confusing code snippet, comment on its purpose.

### Conditional
* Simple if/else: `[ -z "$foo" ] && return 1 || echo "$foo"`
* Ternary operator: `foo=$([ "bar" = "bar" ] && echo "true" || echo "false")`

### Utility scripts¹
* should be sourced to another scripts with `. util.sh`
