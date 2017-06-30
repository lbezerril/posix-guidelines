# posix-guidelines
Guidelines for writing your shell scripts in the POSIX standard

`Version: 1.0.0`
<sub><sup>*[semver.org](http://semver.org/)*</sup></sub>

## Indicate inside your shell scripts that you are using posix-guidelines
Copy and paste in the header of each script:
```sh
# This POSIX sh is following the "posix-guidelines" version="1.0.0" {
#   <https://github.com/lbezerril/posix-guidelines/tree/1.0.0>
# }
```

### Structure
* The first line of your script should be `#!/bin/sh` or `#!/usr/bin/env sh` (Of course, you can include arguments)
* Do not use `;` at the end of the line
* Two spaces should be used as the unit of indentation.
* Avoid long lines. Define a maximum size of columns, and if it exceeds, use `\` to continue on the next line

### Naming convention
* For functions, use underscores with lower case and an underscore prefix, as in `_foo_bar()`
* For [functions that will be sourced](# "Using . in another shell script, to execute commands from a file in the same shell."), use underscores with lower case and a double underscore prefix, as in `__foo_bar()`
* For constants, use underscores with upper case, as in `CONSTANT_NAME="value"`
* For [constants that will be sourced](# "Using . in another shell script, to execute commands from a file in the same shell."), use underscores with upper case and a double underscore prefix, as in `__CONSTANT_NAME="value"`
* For variables, use underscores with lower case, as in `variable_name="$1"`

### Functions
* Use `{ }` for functions, as in `_foo() {}`
* Use `( )` (subshell) for [functions that will be sourced](# "Using . in another shell script, to execute commands from a file in the same shell."), as in `__bar() ()`. This prevents variables declared in the function from being sourced.

### Variables and Constants
* Constants should be declared at the beginning of a script or function.
* Do not use `local` on variables within [functions that will be sourced](# "Using . in another shell script, to execute commands from a file in the same shell.") (subshell)
* Avoid `local`, use only if necessary. Remember that `local` is not part of the POSIX standard, although most POSIX-compliant shell support it. One suggestion is to implement and use the "scope function", as suggested by "user7620483" found [here](https://stackoverflow.com/questions/18597697/posix-compliant-way-to-scope-variables-to-a-function-in-a-shell-script#answer-42452641). However, this may not work if there are readonly variables. So it's up to you to decide!

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
* If you need to execute only one statement within the true and / or false condition, use `&&` and `||`. Otherwise, use `if` `else` syntax. Examples:
```sh
# Using && and ||

[ -z "$foo" ] && echo "bar"

[ -t 0 ] || read stdin

[ -z "$bar" ] && return 1 || echo "$bar"


# Using if, else

if [ -z "$foo" ]; then
  echo "bar"
  return 1
fi

if ! [ -t 0 ]; then
  read stdin
  echo "done"
fi

if [ -z "$bar" ]; then
  return 1
else
  echo "$bar"
  return 0
fi
```
* Ternary operator:
```sh
equals=$([ "$foo" = "$bar" ] && echo "true" || echo "false")
```

### [Sourced scripts](# "Using . in another shell script, to execute commands from a file in the same shell.")
* Use `unset -f` after using functions you do not want to be sourced
* Use `unset -v` after using constants you do not want to be sourced
