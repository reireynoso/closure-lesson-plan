# Review of Exercises

## Exercise 1: Multilingual Greeter 👋🏼
Google translate is down and we want to say hello to people in various languages. Let's create a function, `multiGreeter()`, that will take in a parameter, `language`. Inside of the function, we want the following:
- returns an `anonymous function` that takes in a parameter of `name` and returns a specific greeting statement based on the `language` parameter. I.e if the `language` is `english`, it should return `Hello Name`
- To determine which statement to return, utilize `if` statements.
- Here are samples to work with: `English: Hello, Spanish: Hola, Japanese: Kon'nichiwa, Tagalog: Kamusta`

Solution
```js
function multiGreet(language){
    return function(name){
        if(language === "english"){
            return "Hello " + name
        }

        if(language === "spanish"){
            return "Hola " + name
        }

        if(language === "japanese"){
            return "Kon'nichiwa " + name
        }

        if(language === "tagalog"){
            return "Kamusta " + name
        }
    }
}

const greetEnglish = multiGreet("english")
const greetSpanish = multiGreet("spanish")
const greetJapanese = multiGreet("japanese")
const greetTagalog = multiGreet("tagalog")
console.log(greetEnglish("Bob")) // Hello Bob 
console.log(greetSpanish("Bob")) // Hola Bob 
console.log(greetJapanese("Bob")) // Kon'nichiwa Bob
console.log(greetTagalog("Bob")) // Kamusta Bob  
```

### Review
- Everytime we called the `multiGreet()` function, we created a new `execution context`. Each of those context has their own memory space with their own variables.
- For example, when calling `multiGreet("english")`, it creates an `execution context` where the value of the parameter, `language`, is `english`
- When `greetEnglish("Bob")` is invoked, it has its own reference to the outer variable forming its own closure.
- Even though the four functions lexically sit inside the same `multiGreet()` function, they're going to point at four different spots in memory becuase they were created during four different execution contexts.


## Exercise 2: Bank Account Emulator 🏦
We're tired of using Bank apps to keep track of our 💵💵. We want to create a function, `bankAccount()` that does that job for us! When we invoke the function, we have to pass in a parameter, `initialBalance`. Inside of the `bankAccount()` function, we want the following:
- has an inner function, `changeBy()`, that changes that value of that `initialBalance`,
- returns an object with keys, `deposit`, `withdraw`, and `checkBalance`. The keys `deposit` and `withdraw` will both have a value pointing to an `anonymous function` that takes in a parameter, which will be an `integer`, and that function calls `changeBy()` passing in the parameter it received indicating how much to change the `initialBalance` by. The `checkBalance` will also have a value pointing to an `anonymous function` that will return our `initialBalance` parameter.
- Hint: when `withdrawing`, consider multipying the passed in parameter by `-1`

Solution
```js
function bankAccount(initialBalance){
    function changeBy(num){
        initialValue += num;
    }

    return {
        deposit:function(value){
            changeBy(value)
        },
        withdraw:function(value){
            changeBy(value * -1)
        },
        checkBalance:function(){
            return initialBalance;
        }
    }
}

const makeBank = bankAccount(4000)
console.log(makeBank.checkBalance()) // 4000
console.log(makeBank.deposit(400)) 
console.log(makeBank.withdraw(3000))
console.log(makeBank.checBalance()) // 1400 
```

### Review
- When we first invoke the `bankAccount()` function with the `initialBalance` of 4000, we create an `execution context`, which the variable `makeBank` points to. The `bankAccount()`returns an object with values pointing to methods. 
- Let's examine, `makeBank.checkBalance()`. The method's job is to return the `initialBalance`. That variable is not available in it's scope, so it goes down the scope chain until it notices it as the parameter. That parameter is available in memory due to the `execution context` and due to lexical scoping.
- `makeBank.deposit(400)` will take a similar approach where it first calls the function `changeBy(num)` and that function can't find the variable in it's current scope and decides to look for it. It finds it changes the value of `initialBalance` in memory. `makeBank.withdraw(3000)` takes an identical approach.
- And when we check the balance again, the value returned is less. Why? We were making the changes to the same `intitialBalance` in memory.
- If we were to create another sample:
```js
const anotherBank = bankAccount(2000)
console.log(anotherBank.checkBalance()) // 2000
console.log(anotherBank.deposit(400)) 
console.log(anotherBank.checkBalance()) // 2400 
```
- We would be creating another `execution context` that has its own space in memory. It's completely independent from the `makeBank`.

## Exercise 3: Profile Creator 📱
Tired of using social media apps, but still would like a way of knowing people's basic info? Let's create the next best thing! Create a function, `createProfile()` that will take in three parameters: `name`, in `string`, `bio`, also in `string`, and `age`, in `integer`. When we invoke `createProfile()`, the following will be within the function:
- the function returns an object with keys, `changeName`, `changeBio`, `changeAge`, and `viewProfile`, with all of their values pointing to an `anonymous function`. 
- With the `anonymous functions` of `changeName`, `changeBio`, and `changeAge`, they are each taking a parameter when invoked and we are changing the respective property values on the `information` object.
- The `anonymous function` of `viewProfile` will not need a parameter but it will return an object, containing the key-value pairs of `name, bio, age`, that we passed in as parameters, when invoked.

Solution
```js
function createProfile(name, bio, age){
 
    return {
        changeName: function(newName){
            name = newName
        },
        changeBio: function(newBio){
            bio = newBio
        },
        changeAge:function(newAge){
            age = newAge
        },
        viewProfile:function(){
            return {
                name: name,
                bio: bio,
                age: age
            }
        }

    }
}

const myProfile = createProfile("John", "My name is John", 42)
myProfile.changeName("Jonathan")
myProfile.changeBio("I'm now Jonathan")
myProfile.changeAge(14)
myProfile.viewProfile() // {name: "Jonathan", bio: "I'm now Jonathan", age: 14}
```

### Review
- When we invoke `createProfile("John", "My name is John", 42)`, we are creating it's own `execution context`. 
- This time, we are passing in three parameters that all occupy their own space in memory. 
- Each method within the object being returned has access to all three parameters due to lexical scoping. Even though the parameters themselves are not available locally within each function, they go down the scope chain to look for the variable.
- Instead of creating a separate variable for each parameter, we are directly changing the value and taking advantage of closure. 
- If we were to create another profile: `const anotherProfile = createProfile("Joel", "My name is Joel", 45)`, those parameters have their own `execution context`. Hence, their own space in memory. Changing the parameter values in `myProfile` will not have an effect on `anotherProfile`

## Bonus: Reminder Function 🔖
Tend to forget things frequently? Don't fret! We will design a function, `remindMe()` that will remind us to do certain things! As part of this function, we will implement `setTimeout()` which will take two arguments, the first being the `callback` function and the second, the time for it to execute the `callback` function. Ex. `setTimeout(function(){console.log('hey')}, 3000)`

Quick explanation of what a `callback` is: a function you give to another function, to be run when the other function is finished. The function you call, invoke, 'calls back' by calling the function you gave it when it finishes.

When `remindMe()` is invoked, a two parameters will be expected: `remindMe(reminder, time)`. The first argument will be `reminder`, a `string` message of what the reminder is and the second will be the time, an `integer` of when to remind us. Within the function itself,
- has an inner function, `reminderFunction()` that returns `Reminder: Reminder that you passed in a parameter`
- has a `setTimeout()` that takes in the `anonymous callback function` and the `time` parameters.
- Inside of the `anonymous callback function`, it will invoke `reminderFunction`. NOTE: there's a way to make this shorter. It's okay if you're not aware. You can stick to this approach.

Solution
```js

function remindMe(reminder, time){
    function reminderFunction(){
        return "Reminder: " + reminder
    }
    setTimeout(function(){
        reminderFunction()
    }, time)
}
console.log(remindMe("Eat", 3000)) // After 3 seconds, logs out: Reminder: Eat.
```

### Review
- When we invoke `remindMe()`, we are setting an inner function, `reminderFunction()` as well as calling a `setTimeout()` that will call `reminderFunction()` after the time passed in.
- This is a great demonstration of the power of closures. 
- `setTimeout()` will call `reminderFunction` after a certain amount of time. Yet `reminderFunction` will still have access to the `reminder` parameter passed in due to lexical scoping and scope chain.
- An `execution context` is created when `remindMe()` is invoked and the variables and its environment are stored in memory.