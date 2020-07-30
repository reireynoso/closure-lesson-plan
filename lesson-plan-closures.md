# Topic: Closures in Javascript

## Introduction 👋🏼

Greetings curious learner! Welcome to a notorious topic in Javascript, understanding `closures`. Closures is a Javascript topic that's absolutely vital to understand when advancing with Javascript concepts. However, it can be notoriously difficult to understand, but you're ready 💪. At this point, we've been exposed to the idea of the `execution stack`, and the `execution context`. We will apply those concepts in helping us understand the idea of `closures`. 

A `closure` is a function that references variables in the outer scope from its inner scope. The `closure` preserves the outer scope inside its inner scope. Whoa, what does that even mean? Furthermore, how does that even help me? In plain English, a `closure` is simply a function defined another function. But the real power is the fact that the inner function remembers the environment in which it was created meaning it has access to the outer function's variables and parameter when it was created.

To start with, let's say we have the following code:

```js
function greet(whattosay){
    return function (name){
        console.log(whattosay + ' ' + name)
    }
}

const sayHi = greet("Hello")
sayHi('Person') 

```

Let's examine what's happening. We have a function, `greet()` that accepts a parameter, `whattosay`. This function is returning an `anonymous` function also takes in a parameter, `name`, and that function logs out the `whattosay` and the `name`. We invoke the function, `greet("Hello")`, and store it inside a variable, `sayHi`. The variable, `sayHi` is now the `anonymous` function returned by `greet()`. We invoke `sayHi("Person")` to log out the statement. What do you think will print to the console?

- `Hello Person` ?
- `undefined Person` ?

Not really a trick question! We should get what we originally wanted which is `Hello Person`. But the better question would be, why does it work the way we want it to? 🤔

## How does Closure Work?

### Lexical Scoping

Before we dive straight into how `closures` work, let's first examine an important topic to aid in our understanding, `lexical scoping`. Consider the following code:

```js

let name = "Bob";

function hello(){
    let greeting = "Heya";

    function displayGreeting(){
        console.log(greeting + ' ' + name)
    }
    displayGreeting();
}

hello()
```

In the code, the variable, `name`, is a global variable. It is accessible from anywhere. When we invoke the function, `hello()`, it is creating a local variable name, `greeting` and function called `displayGreeting()`. The `displayGreeting()` is an inner function that is only accessible inside of the `hello()` function. If we try to access it outside of the `hello()` function, we will get an error. The Javascript engine uses the scope to manage the variable accessibility.

According to `lexical scoping`, the scopes can be nested and inner functions can access the variables/functions in its outer scope. So when we invoke `displayGreeting()` within the `hello()` function, the `console.log()` executes and can access the variables, `greeting` and `name` since they are defined in the outer scope.

### Scope Chain

There is one more concept we need to look at and that is `scope chain`. We mentioned about `lexical scoping` and how all inner functions have access to variables defined outside of them. But which variable would it refer to if we have identical named variables? Note this is not ideal or considered good practice to have similar named variables but it can happen. Consider the following code:

```js

let firstNumber = 200;

function getThatNumber(){
    let firstNumber = 500;

    function findNum(){
        console.log('This is your number: ' + firstNumber)
    }
    findNum()
}

getThatNumber()
```

In this code, we have a global variable, `firstNumber`. We have a similar process where we invoke the function, `getThatNumber()` and it creates another `firstNumber` variable inside of its context as well as a `findNum()` function. Once `findNum()` is invoked, what do you think will it `console.log()`? According to `lexical scoping`, both `firstNumber` variables are accessible but which would it actually use? First, the function `findNum()` realizes that the variable isn't defined in its scope. So it goes out of this scope, meaning one layer out. In this case, that would be the context of `getThatNumber()` function. This context does include a variable, `firstNumber`. The variable is now found and the search is stopped. `Scope chain` is the process of moving down the `execution stack` but lexically until the `global execution context` to find it.

### Closures under the Hood

Now that we've covered `lexical scope` and `scope chain`, let's go back to our first example with `greet()` function. When we call `sayHi('Person')`, we're not actually passing in the parameter for `whattosay`. So how would it know that the value is `Hello`?

Let's visually examine what's happening in terms of the `execution stack`, `execution context`, and `lexical scoping`. When this code starts, we have our `Global Execution Context`. When we hit the line, `var sayHi = greet("Hello")`, it invokes the `greet()` function and new `execution context` is created and added to the `execution stack`. The variable passed to it, `whattosay`, is sitting in its variable environment. It returns a new function object. And that's it. The `greet execution context` is then popped off the stack. 

![Closure Beginning](/sample-images/closure-beginning.png)

Before,we mentioned that every `execution context` has this space in memory, where the variables and functions created inside of it live. What would happen to that memory space when the `execution context` goes away? The memory space would be sitting somewhere in memory. 

Now we're in the `global execution context`, and we then invoke the function, `sayHi()` which is pointing to the anonymous function. Once we do, we create another `execution context`. We passed a `variable`, `Person`, that will end up in memory. But when we hit the line, `console.log(whattosay + ' ' + name)`, the Javascript engine sees `whattosay`. What does the engine do? We've learned this before and it's the idea of `lexical scoping`. In other words, it goes to the next point outside where the function was created to look for that variable, since it couldn't find it inside the function itself. Even though the `execution context` of that function `greet()` is gone, and was popped off the stack, the `sayHi() execution context` still has a reference to the variables and memory space of its outer variables.

![Closure Middle](/sample-images/closure-middle.png)

Think about this for a second...

`greet() execution context` is gone but what's in memory for that `execution context` is not and the Javascript engine makes sure that the function can go down the scope chain to find it, even though it's not on the `execution stack` anymore. This way, we say that the `execution context` has closed in its outer variables, the variables that it would normally have reference to. This is the idea of `closures`. It isn't something that you create. It is a feature of the Javascript language.

![Closure End](/sample-images/closure-end.png)

### Function Factories

Now that we have some background on how `closures` work, let's examine how `closures` can be used to our advantage.

### Closures and Callbacks


