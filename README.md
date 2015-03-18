# CoffeeScript Conventions

> My personal CoffeeScript conventions documented for fellow programmers

Some, but not all, of these can be enforced via
[coffeelint](https://github.com/clutchski/coffeelint)

## Whitespace

- Two spaces is an indent. Period.

## Naming

- __Classes__ use `PascalCase` with no inner capitals, e.g. `HtmlBuilder`
- __Functions__ and __variables__ use `camelCase` with no inner capitals, e.g.
  `htmlBuilder`
- __Constants__ use `CONSTANT_CASE`
- The only shorthand variable names allowed:
  - YES: the EventObject `e`
  - YES: `i` for index
  - YES: `el` for element
  - YES: temp objects `args` and `params` 
  - YES: `max`, `min`
  - SOMETIMES: `w`idth, `h`eight
  - NO: `res` (`result`? `response`? `resource`? Too ambiguous!)
  - NO: `prop` for `property` or `mgr` for `manager`, etc.
  - NO: `tmp` for `temporary` -- you shouldn't have these ever.

- __jQuery Objects__ should be prefixed with a $, e.g. `$thing`

### In classes

- Use `_underscorePrefixes` to denote a private member.
  - A `_privateProperty` should only be accessed via setters and getters.
  - A `_privateMethod` is typically used in the case of a [pure
    function](http://www.sitepoint.com/functional-programming-pure-functions/)

## Syntax

### Loops and conditions

- CoffeeScript style is okay when it all fits in 80 characters.

```CoffeeScript
myThing = if case1 is true then 'something short' else 'nothing'
doThat() while size < MAX_SIZE 
```

- Consider a more specific iterator from lodash when other transformations
  or chaining may be a possibility.

- Always use literal `and`, `or`, `isnt`, `not` from CoffeeScript

### Naming

- For booleans, prefix with `is` or `has` such that the variable or function is
  readable in plain English:

```CoffeeScript
isInitialized = true
hasChildren() # function returns a boolean
```

### Variables

- For variables that are objects or arrays, show the braces:

```CoffeeScript
myObject = {
  somekey: true
}

myArray = [
  'item1'
  'item2'
]
```

- Use positive voice for variables -- never do this:

```CoffeeScript
isNotFinished = false
```

- Avoid temp variables. If you have a `for` loop with an `i` index where the
  index isn't used anywhere, you can probably use functional programming
  instead (`map`, `filter`).

- Don't use `on`, `off`, `yes`, and `no` booleans from CoffeeScript

- Space only to the right of colon in a `key: value`
 
- Use string interpolation when only one variable is to be interpolated.

```CoffeeScript
# Okay to use interpolation for one variable. Note double quotes.
myLongText = "Okay then, #{username}"

# Or use concatenation. Note single quotes since not interpolating.
myOtherLongText = 'Good job, ' + #{username}

# Use joins for multiple variables
myLongerText = [
  'Okie dokie '
  username
  ', you can have '
  amount
  ' pieces!'
].join('')
```

- Use `@` for `this`, except where its context changes:
```CoffeeScript
class MyClass

  isVisible: true

  hideLis: ->
    return if @isVisible

    $('li').each ->
      $(this).hide()  # explicitly using `this` helps denote that we're in
                      # a different context

    return
```

### Function arguments

- Omit empty parens:

```CoffeeScript
myFunction = ->
  return
```

- No space between params and arrow:

```CoffeeScript
myFunctionWithParams = (params)->
  return
```
 
- For functions that take more than three arguments or more than one boolean,
  pass an object instead.

```CoffeeScript
args = {
  isTooMany: true
  isHardToRemember: true
}
myFunc(args)
```

### Function calls

Mostly to avoid confusion by non-CoffeeScripters

- For single line function calls, use explicit parentheses:

```CoffeeScript
myFunction(param1, param2)
```

- For multi-line function calls (containing an object argument or
  inline-callback):

```CoffeeScript
myFunction({
  somekey: true
}, moreArgs)

myFunction true, true, (callbackArgs)->
  doSomething()
  return
```

- Callbacks longer than 4 lines should be declared as function handlers:

```CoffeeScript
# creating the (anonymous) handler
callbackHandler = (args)->
  # function body
  return

# calling the function that accepts a callback
myFunction(param1, callbackHandler)
```

### Returns

- Always explicitly `return` from a function for clarity.

### Commenting

- `# Prefer single line comments` over

```CoffeeScript
###
Block comments
###
```

- Document properties, methods, functions using
  [Codo](https://github.com/coffeedoc/codo)

- You can skip commenting if the function name makes the purpose super obvious
  with regards to the function purpose (and/or return type)

- Separate function concerns (as opposed to technical concerns -- which are
  combinations of mixins as defined by Codo) in a class with
  a comments section.

```CoffeeScript
# Base class for things
#
class BaseThing

  methodB: ->
    return false


# A thing
#
# @see {http://google.com}
# @example Does a thing
#
class Thing extends BaseThing

  # @note don't use this
  # @deprecated
  @somePrototypeProperty = 'GO'

  # @property {array<boolean>} _privateProperty some flags
  _flags: [ true, false ]

  constructor: ->
    return @

  ############################################################################
  # Getters - this section has functions related to getting properties.
  ############################################################################

  # getFlagAt
  # 
  # @return {boolean}
  getFlagAt: (index)->
    return true if window.ENABLE_ALL_FLAGS
    return _flags[index]

  ############################################################################
  # Methods - optional section
  ############################################################################

  # methodA
  #
  # @return {boolean}
  methodA: (override)->
    return true if override?
    return getFlagAt(0)

  # methodB
  #
  # @overload BaseThing.methodB
  # @return {boolean}
  methodB: ->
    return getFlagAt(0)

  ############################################################################
  # Facade - basically these methods use the other methods with some business
  # logic
  ############################################################################

  # doMethods
  # 
  # Facade to perform all methods
  #
  # @return {boolean}
  doMethods: ->
    return methodB() if methodA()
    return false

```

## Principles

- Use functional programming -- `map`, `reduce`, `filter`, etc. whenever
  possible.
- Aim for immutability, let the garbage collector do its job.
- Use pure functions instead of maintaining a ton of state.

