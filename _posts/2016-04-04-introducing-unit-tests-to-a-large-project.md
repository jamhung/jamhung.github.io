
At work, our iOS team recently began making real progress on 
introducing unit tests into the main codebase, which contains over 250,000 lines of code,
some written a day ago and some written 5 years ago.

You may be asking, how does a large project go so long without having
unit tests? Well, there are a lot of reasons and excuses for that, but it's a story for another post.

However, if you're in a similar situation as our team was, hopefully I can
provide some helpful suggestions and things to consider before embarking on this noble mission:

<dl>
  <dt>1. Evaluate all of your options</dt>
  <dd>If your project is 100% Objective-C, ask yourself if Swift will start to make its way into your codebase. If that's the case, your team may want to choose a unit testing framework that's compatible with both languages, like <a href="https://github.com/Quick/Quick">Quick</a></dd>

  <dd>Or, maybe your team would prefer to not use 3rd party dependencies and just stick with the built-in XCTest Framework. Does your team like the idea of <abbr title="Behavior Driven Development">BDD</abbr> style tests?</dd>

  <dd>At first, you may find it difficult to answer these questions confidently. I've found it quite useful to begin creating <strong>new Xcode projects as sandboxes to play with these concepts and frameworks</strong> in order to find the best direction for the team and project.</dd>

  <dt>2. Determine the necessary changes to code architecture</dt>
  <dd>The ease in which effective unit tests can be written against your project hinges heavily on the code architecture.

  <dd>Ask yourself if the current architecture allows for stub and mock objects to be easily injected into classes. There are many strategies for refactoring your code to use Dependency Injection.</dd>

  <dd>Is there a clear separation between view and business logic? Massive view controllers are difficult to unit test because they tend to have methods that contain both types of logic mixed together. It may be helpful to consider architectures such as <abbr title="Model View View-Model">MVVM</abbr>, <abbr title="Model View Presenter">MVP</abbr>, or <abbr title="View Interactor Presenter Entity Router">VIPER</abbr> to better separate view and business logic.</dd>

  <dt>3. Start simple</dt>
  <dd>Once you have a better idea of how you want to begin adding unit tests, choose a class that will be simple to add tests to. By this, I mean the class isn't too complex and it won't require a complete overhaul of the code.</dd>

  <dd>As you become more familiar with writing tests and you start to polish your testing methodology, it will be more clear to you how to break apart and refactor existing code in order to effectively test it.
  </dd>

  <dt>4. Create testing guidelines</dt>
  <dd>Throughout this process, it can be a good idea to agree on and document some standards for writing tests.</dd>

  <dd>An example of some standards could be to always add tests for methods containing business logic, to be more lax about unit tests for view related methods as they tend to be more brittle, and to open up classes to mock inputs by refactoring the init method to allow dependency injection.
  </dd>

  <dd>In general, it's a good idea for the team to agree as whole on a set of standards when writing unit tests within the project.</dd>
</dl>

<p>I've listed above some general considerations when introducing unit tests to a large codebase. In future posts, I'll look to provide some more in-depth details and code samples on strategies I've learned for writing effective unit tests.</p>


Until then, Happy Testing!
