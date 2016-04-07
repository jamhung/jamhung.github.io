---
layout: post
title: Dependency injection and unit tests
---

Dependency injection is a design pattern I've found essential to writing effective unit tests. It's a way for me to structure my code so that when I'm testing a specific method, I'm able to replace any of its external dependencies with a stub. Let's go through an example.


## Example setup
`UserDataSource` has a method `fetchFirstUser` that I'd like to unit test. Within the method, there is a dependency on `UserAPIService`.


{% highlight swift %}

class UserDataSource {

    func fetchFirstUser() -> User {
    	let users = UserAPIService.fetchUsers()
    	return users.first
    }
}

{% endhighlight %}

To setup the unit test, it would make sense to stub out the call to `UserAPIService.fetchUsers()` for a couple of reasons:

1. We don't want our test to rely on the actual behavior of this external dependency.
2. Calling this actual method could trigger a database fetch or a blocking network request that would unecessarily slow down our unit test.

Let's use dependency injection so we can introduce a `UserAPIService` stub in our unit test.

## Applying dependency injection

There are a few strategies to refactor the code to allow for dependency injection.

We can modify the method to take a `UserAPIService` object as a parameter. Below, we're also using Swift's default parameter value functionality so any calling code is not required to pass in a boilerplate `userService` argument.

{% highlight swift %}

class UserDataSource {

    func fetchFirstUser(userService: UserAPIService = UserAPIService()) -> User {
        let users = userService.fetchUsers()
        return users.first
    }
}

{% endhighlight %}

## Writing unit tests

Now that our code supports dependency injection, it's easy to write a unit test by creating a `UserAPIServiceStub` class that subclasses `UserAPIService` and overrides the `fetchUsers()` method to return stubbed data.

I've been writing my unit tests with the help of <a href="https://github.com/Quick/Quick">Quick</a> and <a href="https://github.com/Quick/Nimble">Nimble</a>, which prescribe a <abbr title="Behavior Driven Development">BDD</abbr> testing approach I've been enjoying. Check it out!

{% highlight swift %}

describe("fetchFirstUser") {

    // Setup stub class to be injected
    class UserAPIServiceStub: UserAPIService {

        // Override to return stub data
        override func fetchUsers() -> [User] {
            return [
                User(id: 123),
                User(id: 789)
            ]
        }
    }
    
    // Setup the data source object to run the methods we want to test
    var userDataSource: UserDataSource!
    beforeEach {
    	userDataSource = UserDataSource()
    }

    context("when fetchFirstUser is called") {
    	it("should return the first user") {
    	    let stubService = UserAPIServiceStub()
    	    let firstUser = userDataSource.fetchFirstUser(stubService)
    	    expect(firstUser.id) == 123
    	}
    }
}

{% endhighlight %}

## Moar dependency injection

Up above, I showed just *one* way to introduce dependency injection, by modifying the method signature to include additional parameters. Here are some other ways.

1.) If the dependency is used throughout the class, you can create a property to store it and inject it during  initialization.  
{% highlight swift %}

class UserDataSource {
    var userService: UserAPIService

    init(userService: UserAPIService = UserAPIService()) {
    	self.userService = userService
    	super.init()
    }

    func fetchFirstUser() -> User {
        let users = self.userService.fetchUsers()
        return users.first
    }

    func fetchLastUser() -> User {
    	let users = self.userService.fetchUsers()
    	return users.last
    }
}
{% endhighlight %}

2.) If you prefer not to modify the public interface of the class, you can move the dependency out to an internal computed property or method.

{% highlight swift %}

class UserDataSource {
    let userService: UserAPIService {
        return UserAPIService()
    }

    func fetchFirstUser() -> User {
        let users = self.userService.fetchUsers()
        return users.first
    }
}
{% endhighlight %}

Although this is not quite dependency injection, it enables you to subclass `UserDataSource` and override the `userService` getter to return a stub service for unit testing.

Hopefully these quick and easy strategies for introducing dependency injection make it easier for you to start adding unit tests to your code.