# 如何摇滚🤟通过理解 JavaScript 回调、ES6 承诺和 ES7 异步/等待进行异步调用🔥😎

> 原文：<https://dev.to/themarcba/how-to-rock-asynchronous-calls-by-understanding-javascript-callbacks-es6-promises-and-es7-async-await-4nb3>

*交叉发布自[developer . blog](https://developer.blog/async-await)T3】*

在用 JavaScript 编程时，回调可能是非常有用的东西，但是当大量使用时，编码可能会变得混乱。这篇文章解释了现代 JavaScript 规范中的承诺和异步/等待是如何工作的，以及它们如何提高代码的可读性。

在这篇文章中，我将使用箭头函数，你可以阅读我的[箭头函数博客文章](https://developer.blog/es6-arrow-functions)的第一章。

## 回调

JavaScript 最棒的一点是函数被视为对象。这使得将函数作为参数传递给另一个函数成为可能，然后该函数可以调用内部传递的函数。传递的函数称为回调函数。

这在处理 sun 异步的任务时很方便，我们不能确定任务的确切完成时间，所以我们可以处理结果数据。一个真实的例子是从 REST API 请求数据。

下面是一个函数的传统回调的例子——出于演示的目的——需要 2 秒钟将两个数相加:

```
// Definition of the asynchronous function
const add = (a, b, callback) => {
    setTimeout(() => {
        const result = a + b
        callback(result)
    }, 2000);
}

// Calling the asynchronous function and passing the callback function
add(3, 6, sum => {
    // Execute this when result is ready
    console.log(`The sum is: ${sum}`)
}) 
```

Enter fullscreen mode Exit fullscreen mode

当您执行该代码时，将调用 add 函数，两秒钟后，将执行回调函数，并显示结果(记录到控制台)。

看起来没那么糟，对吧？但是有两件事使这种方法使用起来令人生厌:

*   当试图引入错误处理时(出错了)
*   当试图一个接一个地使用各种回调函数时

### 错误处理

让我们假设我们虚构的函数只能添加正数。我们希望用户知道在试图处理负数时出现了问题。

```
const add = (a, b, callback) => {
    setTimeout(() => {
        // Checking if the input numbers are right
        if(a >= 0 && b >= 0) {
            const result = a + b
            callback(result)
        } else {
            // Passing an error if there is a negative input
            callback(undefined, 'Numbers must be non-negative')
        }
    }, 2000);
}

add(3, -6, (sum, error) => {
    // If an error occured in the add function, display it
    if(error) {
        console.log(`An error occured: ${error}`)
    } else {
        console.log(`The sum is: ${sum}`)
    }
}) 
```

Enter fullscreen mode Exit fullscreen mode

### 链接

一个接一个地执行各种回调(链接)，或者被称为“*回调地狱*”，会很快变得非常混乱。

假设我们想要计算结果总和的平方，然后检查这个平方是奇数还是偶数。每次多花 1 秒钟执行。

```
const add = (a, b, callback) => {
    setTimeout(() => {
        // Checking if the input numbers are right
        if(a >= 0 && b >= 0) {
            callback(a + b)
        } else {
            // Passing an error if there is a negative input
            callback(undefined, 'Numbers must be non-negative')
        }
    }, 2000);
}

const tripleDown = (a, callback) => {
    setTimeout(() => {
        callback(a * 3)
    }, 1000);
}

const isEven = (a, callback) => {
    setTimeout(() => {
        callback(a % 2 === 0)
    }, 1000);
}

add(3, -6, (sum, error) => {
    // If an error occured in the add function, display it
    if(error) {
        console.log(`An error occured: ${error}`)
    } else {
        square(sum, tripleResult => {
            isEven(square, isEvenResult => {
                console.log(`The sum is: ${sum}`)
                console.log(`The triple of the sum is: ${tripleResult}`)
                console.log(`The triple is even: ${isEvenResult}`)
            })
        })
    }
}) 
```

Enter fullscreen mode Exit fullscreen mode

我想我们现在可以同意代码开始变得混乱，这使得一段时间后很难理解和维护。

## 承诺

承诺救援！2015 年，当 ES6 发布时，一个漂亮的小功能被引入，这使得开发者可以逃离回调地狱。

承诺顾名思义就是:承诺在未来的某个时间会有结果。那个结果可以是成功的，那么这个承诺将会被*履行*或者失败，这将会使这个承诺被*拒绝*。虽然(还)没有答案，但承诺是*待定*。

让我们用一个承诺来写我们开始时的代码(两个数字延迟两秒相加的例子)。

```
const add = (a, b) => {
    // Returning a promise that there will be an answer sometime
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            // Resolving the promise
            // This means it was successful
            resolve(a + b)
        }, 2000);
    })
}

// Executing the add function, *then* executing the callback.
add(2, 9).then(sum => {
    console.log(`The sum is: ${sum}`)
}) 
```

Enter fullscreen mode Exit fullscreen mode

当我们创建的承诺正在被*解析*时，`.then()`正在被执行，它将具有在解析调用中传递的任何值。

### 错误处理

处理错误是承诺的乐趣。而不是让回调函数接受额外的参数。

我们不调用承诺中的`resolve()`，而是要调用`reject()`让承诺不成功结束。让我们扩展这个例子，增加不处理负数的限制:

```
const add = (a, b) => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if(a >= 0 && b >= b) {
                // The promise is being fullfilled successully
                resolve(a + b)
            } else {
                // The promise is being fullfilled unsuccessully
                reject('Numbers must be non-negative')
            }
        }, 2000);
    })
} 
```

Enter fullscreen mode Exit fullscreen mode

现在处理这个错误已经很优雅了。我们只是给承诺执行增加一个`.catch()`:

```
add(2, -9).then(sum => {
    // Processing the asynchonous function result
    console.log(`The sum is: ${sum}`)
}).catch(error => {
    // The error has being "caught"
    console.log(`An error occured: ${error}`)
}) 
```

Enter fullscreen mode Exit fullscreen mode

### 链接

现在将各种异步函数链接在一起也更容易了。这里有一个链接三次相同的`add()`函数的例子。先加 2+5，再加那个+ 43 的结果，再加那个+ 1000 的结果。

```
add(2, 5).then(firstSum => {
    console.log('first sum', firstSum);
    return add(firstSum, 43)
}).then(secondSum => {
    console.log('second sum', secondSum);
    return add(secondSum, 1000)
}).then(thirdSum => {
    console.log('third sum', thirdSum);
}).catch(error => {
    console.log('error', error);
}) 
```

Enter fullscreen mode Exit fullscreen mode

这是一种更干净的方式，人们在 2015 年对此感到非常兴奋，因为他们终于可以交付更干净的代码，并将回调地狱踢回原处(常规地狱)。

但是仍然有两个问题:

*   在每个回调的回调中，你不能访问中间的结果(例如，你不能访问第三个`.then()`的`firstSum`
*   将异步函数链接在一起仍然不是那么直观

这两个问题在一年后发布的 ES7 中得到了解决。

## 异步/等待

Async/Await 不是一项新技术，而不是建立在承诺之上的新工具集。它的目的是使异步函数真正易于编码和理解，其语法可以非常自然地从键盘上流出。最棒的是，已经编程的东西将继续与 async/await 一起工作，因为我们只是以不同的方式编写代码，而不是一种新技术。

### 异步

当您将`async`关键字放在函数前面时(不管是 arrow 还是 regular)，它会自动返回一个(已解决的)承诺，而不是返回值。

```
const doAsynchronousStuff = async () => {
    return 4711;
}

// Returns: Promise { 4711 } 
```

Enter fullscreen mode Exit fullscreen mode

### 等待

当在函数调用前使用`await`时，JavaScript *在继续执行下一行之前等待*的承诺被履行。

**`await`只能在`async`函数内部使用！**

让我们看看这个例子(假设来自*的`add`函数承诺>错误处理*已经存在:

```
const doCalculations = async () => {
    const sum = await add(13, 99)
    return sum
}

doCalculations().then(result => {
    console.log(`The result is: {result}`)
}) 
```

Enter fullscreen mode Exit fullscreen mode

### 错误处理

一个`await`函数调用后的下一行是**，只有当承诺被履行**时才会被执行。当它被拒绝时，异步函数中的所有未来执行都将被停止。

有一种方法可以捕捉每个单独的`await`函数调用的错误，使用一个很好的老式 try/catch 语句:

```
const doCalculations = async () => {
    let sum;
    try {
        // Try to execute this...
        sum = await add(13, -99)
    } catch (error) {
        // If something goes wrong, we catch the error here
        console.log(`An error occured: ${error}`);
    }
    return sum
} 
```

Enter fullscreen mode Exit fullscreen mode

### 链接

现在链接比以前更容易了。您编写代码的方式甚至让您相信它们是同步调用，但实际上，所有的`Promise`魔法都发生在幕后。

const doccalculations = async()= > {
const sum = await add(13，-99)
const sum2 = await add(sum，1000)
const sum 3 = await add(sum 2，9999)

```
// You could access all three variables here.
// For example to do comparisons

return sum3 
```

Enter fullscreen mode Exit fullscreen mode

}

## 总结🙌

async/await 现在是一个行业标准，推荐你使用它，因为它给你带来了很多好处。然而，重要的是要知道它从何而来，如何在引擎盖下工作。使用它时，很容易忘记我们实际上是在做异步调用。

现在，您应该已经准备好创建自己的支持 Promise 的库，并以一种简单易读的方式使用已经支持 Promise 的现有库(所有重要的库都支持 Promise)。

*照片由 [Alex 在 Unsplash](https://unsplash.com/photos/OMF0olQno6M) 上拍摄*