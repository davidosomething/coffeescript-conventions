# CoffeeScript Conventions

> My personal CoffeeScript conventions documented for fellow programmers

## Variable naming

- __Classes__ use `PascalCase` with no inner capitals, e.g. `HtmlBuilder`
- __Functions__ and __variables__ use `camelCase` with no inner capitals, e.g. `htmlBuilder`
- __Constants__ use `CONSTANT_CASE`
- The only shorthand variable names allowed:
  - YES: the EventObject `e`
  - YES: temp objects `args` and `params` 
  - NO: `res` (`result`? `response`? `resource`? Too ambiguous!)
  - NO: `prop` for `property` or `mgr` for `manager`, etc.

### In classes

- Use `_underscorePrefixes` to denote a private member.
  - A `_privateProperty` should only be accessed via setters and getters.
  - A `_privateMethod` is typically used in the case of a [pure function](http://www.sitepoint.com/functional-programming-pure-functions/)

## Syntax

### Naming

- For booleans, prefix with `is` or `has` such that the variable or function is readable in plain English:

```
isInitialized = true
hasChildren() # function returns a boolean
```

### Variables

- For variables that are objects or arrays, show the braces:

```
myObject = {
  somekey: true
}

myArray = [
  'item1'
  'item2'
]
```

- Use positive voice for variables -- never do this:

```
isNotFinished = false
```

### Function arguments

- For functions that take more than three arguments or more than one boolean, pass an object instead.

```
args = {
  isTooMany: true
  isHardToRemember: true
}
myFunc(args)
```

### Function calls

Mostly to avoid confusion by non-CoffeeScripters

- For single line function calls, use explicit parentheses:

```
myFunction(param1, param2)
```

- For multi-line function calls (containing an object argument or inline-callback):

```
myFunction({
  somekey: true
}, moreArgs)

myFunction true, true, (callbackArgs)->
  doSomething()
  return
```

- Callbacks longer than 4 lines should be declared as function handlers:

```
# creating the (anonymous) handler
callbackHandler = (args)->
  # function body
  return

# calling the function that accepts a callback
myFunction(param1, callbackHandler)
```

