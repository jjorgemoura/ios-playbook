# Error Handling



## Representing and Throwing Errors

In Swift, errors are represented by values of types that conform to the Error protocol. This empty protocol indicates that a type can be used for error handling.

The common way to modeling a group of related error conditions is with enumerations, with associated values allowing for additional information about the nature of an error to be communicated. Example:

```swift
enum VendingMachineError: Error {
    case invalidSelection
    case insufficientFunds(coinsNeeded: Int)
    case outOfStock
}
```

Throwing an error allows to indicate that something unexpected happened. The way to throw an error is through the `throw` keyword.

```swift
throw VendingMachineError.insufficientFunds(coinsNeeded: 5)
```

When a method is able to throw and error, that method needs to be declared as a method that can throw an error in order to call site knows and be ready to handle that error. The way to announce that a method can throw an error, the kwyword `throws` needs to be present in method declaration.

```swift
func canThrowErrors() throws -> String

func cannotThrowErrors() -> String
```

## Handling Errors

There are four ways to handle errors in Swift. When a function throws an error, the calling code (function) can handle the potential error in one of four ways:

- propagate the error from a function to the code that calls that function
- handle the error using a do-catch statement
- handle the error as an optional value
- assert that the error will not occur

The call sites of every throwing function needs to notice that is calling a throwing function and that something need to be done to handler that error.
 
This is achieved by the use of `do-catch`, `try`, `try?` or `try!`.


### propagate the error

The call function will not handle the error itself, instead, will throw its self and bypass pass the error to its own call function.

```swift
func buyFavoriteSnack(person: String, vendingMachine: VendingMachine) throws {
    let snackName = favoriteSnacks[person] ?? "Candy Bar"
    
    try vendingMachine.vend(itemNamed: snackName)
}
```

So, **propagate the error from a function to the code that calls that function** means the use of `try` to call a throwing function and mark it self has a `throws` function.

**Note:** Not only functions are able to throw errors. Initializers (`init`) can also throw errors.

### handle the error using a do-catch statement

A `do-catch` statement allows the calling site to handle errors by running a block of code. If an error is thrown by the code in the do clause, it is matched against the catch clauses to determine which one of them can handle the error.

```swift
var vendingMachine = VendingMachine()
vendingMachine.coinsDeposited = 8
do {
    try buyFavoriteSnack(person: "Alice", vendingMachine: vendingMachine)
    print("Success! Yum.")
} catch VendingMachineError.invalidSelection {
    print("Invalid Selection.")
} catch VendingMachineError.outOfStock {
    print("Out of Stock.")
} catch VendingMachineError.insufficientFunds(let coinsNeeded) {
    print("Insufficient funds. Please insert an additional \(coinsNeeded) coins.")
} catch {
    print("Unexpected error: \(error).")
}
// Prints "Insufficient funds. Please insert an additional 2 coins."
```

The order of the `catch` statements is important. The error is matched with each statement and, as soon as a match is done, the error will be managed.

So, **handle the error using a do-catch statement** means a thorough error treatment is done. The call site will handle assume and face the treament of the error.

**Note:** A function can have a `do-catch` block and still decide to throw/propagate the error to its calling site if decides it is not able to handle it.

```swift
func nourish(with item: String) throws {
    do {
        try vendingMachine.vend(itemNamed: item)
    } catch is VendingMachineError {
        print("Couldn't buy that from the vending machine.")
    }
    // Every error that is not a "VendingMachineError" will be thrown to call site.
}
```

### handle the error as an optional value

In case we want to considerar an error thrown by function a function as if the function was returning a `nil` value, an error can be "converted" to an optional value using `try?`. If an error is thrown while evaluating the try? expression, the value of the expression is nil.

```swift
func someThrowingFunction() throws -> Int {
    // ...
}

let x = try? someThrowingFunction()

let y: Int?
do {
    y = try someThrowingFunction()
} catch {
    y = nil
}
```

So, **handle the error as an optional value** means an error is as if the function is returning an optional.

### assert that the error will not occur

Sometimes, due to the nature of a specific API, we are confident that a throwing function or method won’t, in fact, throw an error at runtime. On those occasions, we can use `try!` before the expression to disable error propagation and wrap the call in a runtime assertion that no error will be thrown. 

**Note:** Using the `try!` keyword has a similar effect to `Forced Unwrapping`. If an error actually is thrown, a runtime error will happen.

```swift
let photo = try! loadImage(atPath: "./Resources/John Appleseed.jpg")
```

In this case, because the image is shipped with the application, no error will be thrown at runtime, so it is appropriate to disable error propagation.

So, **assert that the error will not occur** means there is confidence an error will never be thown so that possibility is being discarded.

## Specifying Cleanup Actions

If a function needs to perform some "cleaning" after throwing a error, that can be declared through the use for a `defer` block.

```swift
func processFile(filename: String) throws {
    if exists(filename) {
        let file = open(filename)
        defer {
            close(file)
        }
        while let line = try file.readline() {
            // Work with the file.
        }
        // close(file) is called here, at the end of the scope.
        // This is important to balance with the "open(file)" statement
    }
}
```

#### Resources:

https://docs.swift.org/swift-book/LanguageGuide/ErrorHandling.html