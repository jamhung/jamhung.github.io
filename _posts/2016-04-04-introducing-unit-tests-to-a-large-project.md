
At work, our iOS team recently began making real progress on 
introducing unit tests into the main codebase, which contains over # lines of code,
some written a day ago and some written 5 years ago.

You may be asking, how does a large project go so long without having
unit tests? Well, there are a lot of reasons and excuses for that, but it's a story for another post.

However, if you are in a similar situation as our team was, hopefully I can
provide some helpful suggestions and things to consider before embarking on this noble mission:

<dl>
  <dt>1. Evaluate all of your options</dt>
  <dd>If your project is 100% Objective-C, ask yourself if Swift will start to make its way into your codebase. If that's the case, your team may want to choose a unit testing framework that's compatible with both languages, like <a href="https://github.com/Quick/Quick">Quick</a></dd>

  <dd>Or, maybe your team would prefer not having any 3rd party dependencies for testing and just stick with the built-in XCTest Framework. Does your team like the idea of <abbr title="Behavior Driven Development">BDD</abbr> style tests?</dd>

  <dd>Your team may find it difficult to answer these questions confidently. I've found it quite useful to begin creating <strong>new Xcode projects as sandboxes to play with these concepts and frameworks</strong> in order to find the best direction for the team and project.</dd>

  <dt>2. Determine the needed changes to code architecture</dt>
  <dd>The ease in which effective unit tests can be written against your project hinges heavily on the code architecture.

  <dd>Does the current architecture allow for stub/mock objects to be easily injected into classes and their methods? There are many strategies for refactoring code to use Dependency Injection.</dd>

  <dd>Is there a clear separation between view and business logic? Massive view controllers are difficult to unit test because there tends to exist methods that contain both types of logic. It may be helpful to consider architectures such as <abbr title="Model View View-Model">MVVM</abbr>, <abbr title="Model View Presenter">MVP</abbr>, or <abbr title="View Interactor Presenter Entity Router">VIPER</abbr> to create testable classes.</dd>

  <dt>3. Start simple</dt>
  <dd>Once you have a better idea of how you want to start adding unit tests, choose a class that will be easier to add tests to. By this I mean that the class won't require a complete overhaul of the code and it doesn't contain a ton of complexity.</dd>

  <dd>As you become more familiar with writing tests and you start to polish your testing skills, it will be more clear to you how to break apart and refactor existing code in order to effectively test it.
  </dd>

  <dt>4. Create testing guidelines</dt>
  <dd>Throughout this process, it can be a good idea to agree on some team standards for writing tests and to document them.</dd>

  <dd>Some standards could be to always add tests for methods containing business logic, to be more lax about unit tests for view related methods as they might change quite frequently, and to open up testable classes to stub/mock inputs by refactoring the init method to allow dependency injection.
  </dd>

  <dd>These are just some example standards. In general, it's a good idea for the team to agree as whole on a set of standards when writing unit tests within the project.</dd>
</dl>

<p>I've listed above some general considerations when introducing unit tests to a large codebase. In future posts, I'll look to provide some more in-depth details and code samples on strategies I've learned for writing effective unit tests.</p>


Until then, Happy Testing!
