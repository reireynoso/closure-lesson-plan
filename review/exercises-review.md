# Review of Exercises

## Exercise 1: Multilingual Greeter 👋🏼
Google translate is down and we want to say hello to people in various languages. Let's create a function, `multiGreeter()`, that will take in a parameter, `language`. Inside of the function, we want the following:
- returns an `anonymous function` that takes in a parameter of `name` and returns a specific greeting statement based on the `language` parameter. I.e if the `language` is `english`, it should return `Hello Name`
- To determine which statement to return, utilize `if` statements.
- Here are samples to work with: `English: Hello, Spanish: Hola, Japanese: Kon'nichiwa, Tagalog: Kamusta`

Sample (I should be able to make the following calls to your function)
```js
const greetEnglish = multiGreet("english")
const greetSpanish = multiGreet("spanish")
const greetJapanese = multiGreet("japanese")
const greetTagalog = multiGreet("tagalog")
console.log(greetEnglish("Bob")) // Hello Bob 
console.log(greetSpanish("Bob")) // Hola Bob 
console.log(greetJapanese("Bob")) // Kon'nichiwa Bob
console.log(greetTagalog("Bob")) // Kamusta Bob  
```

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
```

## Exercise 2: Bank Account Emulator 🏦
We're tired of using Bank apps to keep track of our 💵💵. We want to create a function, `bankAccount()` that does that job for us! When we invoke the function, we have to pass in a parameter, `initialBalance`. Inside of the `bankAccount()` function, we want the following:
- has an inner function, `changeBy()`, that changes that value of that `initialBalance`,
- returns an object with keys, `deposit`, `withdraw`, and `checkBalance`. The keys `deposit` and `withdraw` will both have a value pointing to an `anonymous function` that takes in a parameter, which will be an `integer`, and that function calls `changeBy()` passing in the parameter it received indicating how much to change the `initialBalance` by. The `checkBalance` will also have a value pointing to an `anonymous function` that will return our `initialBalance` parameter.
- Hint: when `withdrawing`, consider multipying the passed in parameter by `-1`

Sample (I should be able to make the following calls to your function)
```js
const makeBank = bankAccount(4000)
console.log(makeBank.checkBalance()) // 4000
console.log(makeBank.deposit(400)) 
console.log(makeBank.withdraw(3000))
console.log(makeBank.checBalance()) // 1400 
```

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
```

## Exercise 3: Profile Creator 📱
Tired of using social media apps, but still would like a way of knowing people's basic info? Let's create the next best thing! Create a function, `createProfile()` that will take in three parameters: `name`, in `string`, `bio`, also in `string`, and `age`, in `integer`. When we invoke `createProfile()`, the following will be within the function:
- the function returns an object with keys, `changeName`, `changeBio`, `changeAge`, and `viewProfile`, with all of their values pointing to an `anonymous function`. 
- With the `anonymous functions` of `changeName`, `changeBio`, and `changeAge`, they are each taking a parameter when invoked and we are changing the respective property values on the `information` object.
- The `anonymous function` of `viewProfile` will not need a parameter but it will return an object, containing the key-value pairs of `name, bio, age`, that we passed in as parameters, when invoked.

Sample (I should be able to make the following calls to your function)
```js
const myProfile = createProfile("John", "My name is John", 42)
console.log(myProfile.changeName("Jonathan"))
console.log(myProfile.changeBio("I'm now Jonathan"))
console.log(myProfile.changeAge(14))
console.log(myProfile.viewProfile()) // {name: "Jonathan", bio: "I'm now Jonathan", age: 14}
```

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
                name,
                bio,
                age
            }
        }

    }
}

const myProfile = createProfile("John", "My name is John", 42)
myProfile.changeName("Jonathan")
myProfile.changeBio("I'm now Jonathan")
myProfile.changeAge(14)
myProfile.viewProfile()
```

## Bonus: Reminder Function 🔖
Tend to forget things frequently? Don't fret! We will design a function, `remindMe()` that will remind us to do certain things! As part of this function, we will implement `setTimeout()` which will take two arguments, the first being the `callback` function and the second, the time for it to execute the `callback` function. Ex. `setTimeout(function(){console.log('hey')}, 3000)`

Quick explanation of what a `callback` is: a function you give to another function, to be run when the other function is finished. The function you call, invoke, 'calls back' by calling the function you gave it when it finishes.

When `remindMe()` is invoked, a two parameters will be expected: `remindMe(reminder, time)`. The first argument will be `reminder`, a `string` message of what the reminder is and the second will be the time, an `integer` of when to remind us. Within the function itself,
- has an inner function, `reminderFunction()` that returns `Reminder: Reminder that you passed in a parameter`
- has a `setTimeout()` that takes in the `anonymous callback function` and the `time` parameters.
- Inside of the `anonymous callback function`, it will invoke `reminderFunction`. NOTE: there's a way to make this shorter. It's okay if you're not aware. You can stick to this approach.

Sample (I should be able to make the following calls to your function)
```js
console.log(remindMe("Eat", 3000)) // After 3 seconds, logs out: Reminder: Eat.
```

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
```