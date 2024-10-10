+++
title = 'Dynamic dependency injection for cool kids'
date = 2024-10-10T22:26:23+02:00
draft = false
tags = ["swift"]

[params]
banner = ""
+++

Depedency injection is a big part of the Swift/SwiftUI ecosystem.

## The basic way



## Custom dependency injection

A good start would be to define what our code should look like. Swift `@propertyWrapper` is a nice way to abstract some code and gives us some customization for handling and projecting property values. Here is what our code should look like.

```swift
final class ViewModel {
    @Inject private var dependency: CustomDependency
}
```

Here we only have to define the type of the dependency and the propertywrapper should handle the injection by itself. For dependencies we can create a custom protocol for tagging objects.

```swift
protocol Dependency {}
```

Our basic property wrapper should have a structure like this:

```swift
@propertyWrapper
struct Inject<T : Dependency> {

    init() {
        // resolve dependency
    }

    var wrappedValue: T? // Optional if dependency not found
}
```

Nice, but how are we supposed to retrieve the dependency? A good way would be to have a dependencyManager where we can resolve dependencies. If we create a global shared instance, the inject method has access and can resolve dependencies.

```swift
final class DependencyManager {
    public let shared: DependencyManager = DependencyManager()
    private var dependencies: [ObjectIdentifier : Dependency ] = .init()

    func register<T: Dependency>(_ dependency: T) {
        let id = ObjectIdentifier(dependency)
        dependencies[id]
    }

    func resolve<T: Dependency>(_ type: T.Type) -> T? {
        let id = ObjectIdentifier(type)
        return dependencies[id] as? T
    }
}
```

We store dependencies using an object identifier. This prevents duplicate dependencies.

Going back to our inject structure, we can use the global dependency manager to resolve dependencies.

```swift
@propertyWrapper
struct Inject<T : Dependency> {
    private let dependencyManager = DependencyManager.shared
    init() {
        wrappedValue = dependencyManager.resolve(T.self)
    }

    var wrappedValue: T? // Optional if dependency not found
}
```

## A more dynamic injection system

-- This involves using reflection. When I hear reflection my balls already get sweaty.

## This is the way 🚀

Hey, you've reached the cool kids section. In the previous example we've created a nice dynamic dependency injection system. But we can do better.
