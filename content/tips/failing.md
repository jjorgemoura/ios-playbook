# Failing Swift

Swift gives us several mechanisms to validate data and handle the execution flow differently based on the type of validation we are doing.

There are different degrees of severity when we validate data. Most cases this happens when we need to unwrap optionals.

In some cases we are pretty sure the data is valid but due to the nature of the API, we have an optional that needs to be unwrapped. One very common case is the URL(string:) method, in particular when we provide the url and we are sure the url is valid.

In other situations, it can be a developer error. In this case is fine to crash the app with a fatal error. The sooner we hit the error the better, it gets fixed.

So, when facing Errors that should never happen (or if happens is a devs mistake), the correct approach is crash the app.

## Ways of Failing

In Swift, there are 5 different ways we can use if we want to stop the execution of our software (apart from “abort()” or “exit()”). Those are:

- assert
- assertionFailure
- precondition
- preconditionFailure
- fatalError

One thing that interferes with the way of failing is the “Swift Optimization Levels”.

There are 3 levels of optimisation:

- -Onone (default for Debug)
- -O (default for release)
- -Ounchecked

Note: We should not use “-Ounchecked” unless we are really aware of all the implications.


### assert()

If the condition evaluates false, the execution of the code stops. 

Parameters are:

- condition to verify
- message
- file
- line number

When to use: Should be used internally, privately, as a sanity check tool mainly to catch developers “mistakes”.

From Apple’s documentation:

> Use this function for internal sanity checks that are active during testing but do not impact performance of shipping code. To check for invalid usage in Release builds, see `precondition(_:_:file:line:)`

Visibility: -Onone
This means the assert() command is “invisible” in release builds. Is like that line is not even present (in fact that command is not present in release build).


### assertionFailure()

Similar with asset() but doesn’t have the “condition” parameter (used when the test condition is done outside). It stops immediately.

When to use: same as “assert” but when the test is done outside. Example is we have mode than one command to execute when the validation fails.

Visibility: -Onone


### precondition()

It has, basically, the same behaviour of the assert() however it is still present in -O builds (typically, the release builds). It has same parameters as “assert()”.

From Apple’s documentation:

> Use this function to detect conditions that must prevent the program from proceeding, even in shipping code.

When to use: Ideally should be used in public APIs, in order to validate parameters.

Visibility: -Onone and -O


### preconditionFailure()

Follows the same pattern of predecessors.

When to use: Same as assertFailure().

Visibility: -Onone and -O


### fatalerror()

This is the more extreme command. 

When to use: when something went really wrong.

Visibility: -Onone and -O and -Ounchecked


## Discussion

These 5 five ways of failing seems too similar. Main question still remains unanswered. When, if when, should we “crash” in production.

One possible rationale to follow, based on Swift’s code base (more precisely a comment from Dave Abrahams in a proposal's discussion), is:

– assert: checking our own code for internal errors.
– precondition: checking that external clients have given you valid arguments. This is mainly valid for library’s development.

Not entirely a rule of thumb (because there are exceptions) but we can simplistically think that preconditions should be used in public methods and asserts in private methods.
This also means the preconditions "demands" public documentation.

Another way of viewing errors and ways of failing is if the error is a **recoverable** or a **non-recoverable** error. All these functions are “non-recoverable”.

Another way is to distinguish between Developer Errors vs Execution Errors.

![](https://paper-attachments.dropbox.com/s_53DB3428A03BC13C35C0E8B7ED8BAA791EB00ADC4AB1A843B7E28DF6F34DFED1_1571399190336_Screenshot+2019-10-18+at+12.46.21.png)


## Unit testing 

One tricky point while testing, and in order to get a good coverage, is how to text the “unhappy” path. How can we assert that a “fatalerror” was successfully triggered?!

The solution is via replacing the default fatalError function. In a simple way:

- redefine the function fatalError in our codebase. 
    - This function will intercept all calls, not the default Swift implementation.
- create a struct with 4 static “functions”.
    - variable holding the current fatalError function.
    - a variable with the reference to the default the Swift.fatalError function.
    - a function to set a new fatalError implementation to var holding current fatalError function.
    - a function that restore the default implementation has the current in use.
- Having the body of our definition of the fatalError() pointing for first var in struct defined just before.

We can follow the same approach for the remaining functions (assert(), assertionFailure(), precondition() and preconditionFailure())


### XCTUnwrap

Handling optionals in Swift could be quite verbose. This is particular notorious while testing, where we control the data and we now there is data. In order to help with these cases, we can now  (Xcode 11+) use the function XCTUnwrap().

From Apple’s documentation:

> XCTUnwrap asserts that an optional variable’s value is not nil, returning its value if the assertion succeeds.


## References:

https://www.swiftbysundell.com/articles/picking-the-right-way-of-failing-in-swift

https://ericasadun.com/2015/12/15/interesting-discussions-on-swift-evolution/

https://agostini.tech/2017/10/01/assert-precondition-and-fatal-error-in-swift/

https://blog.krzyzanowskim.com/2015/03/09/swift-asserts-the-missing-manual/

https://medium.com/@marcosantadev/how-to-test-fatalerror-in-swift-e1be9ff11a29
