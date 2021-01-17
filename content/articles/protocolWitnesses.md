# Protocol Witnesses

## Protocols

Protocols are a key tool, nowadays, to handle *dependencies*. Typically, we define the dependency interface, then we have a production conformance and a test conformance.

However, the use of dependencies brings several complexities and challenges to our codebase.

- increase compile time. 
- cause stresses on dev tools. Some dependencies don't work in iOS Simulator or Playgrounds.
- difficult to test. In particular the real implementation

However, these complexities are primally ***accidental*** complexities, something we get by accidental. However, they can be improved and/or resolved and/or managed in a better way. They are not ***essential*** complexities.

Another issue with using protocols to design a dependency and use a live/test comformance limits the way we use SwiftUI and, in particular, the new **Preview** functionality. We need to have always some dummy data to work with in order to render the app in **Preview**. Also, during development, we want a huge controll over our data in order to not depend, 100%, on server. Also, we want to exercise the data to "unexpected" results to test and validate the UI.

### Pros & Cons

**Pros**

- Simple and popular solution.

**Cons**

- Dynamic Dispatch
- Limitation with associated types

Protocols are being overused for when there is a need for only 2 conformances, the live one and the test one. A protocol that only has two conformances is not a strong form of abstraction. There is not a single protocol that Apple gives us that is meant to only have two conformances.

Of course we can always create more "test" conformances, to reflect different states of data (error, weird values, etc). However, this easily becomes quite verbose and difficult to manage. There is an explotion of types. And soon we add more endpoints to our protocol, it breaks multiple types (the conformances)

One possible solution is create a ***Mock of All Mocks***, were we create a test conformance that behaves the way we tell them to behave case by case. Basically this is achieved by adding public variables to allow injecting the data that the endpoints will after use. 

## Protocol Witnesses


### Pros & Cons



### Terminology

Conformance - All the entities (classes or structs or enums) that conforms to a protocol.

Endpoints - Methods or variables in a Protocol. Kind of protocol definitions or constraints.

#### Resources
