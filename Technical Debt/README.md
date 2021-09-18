# Technical Debt
- incurred when we do something the quick way rather than the right way
- speeds you up in the short term but slows down future development
- usually, bigger the project, bigger the technical debt
- check Martin Fowler’s Technical Debt Quadrant for categorizing and prioritizing it
- can be incurred deliberately as a business decision but still needs to be handled
- can also be incurred inadvertently when the team is not aware of it or not experienced enough
- impacts quality, maintainability, productivity and testability

## Types of Technical Debt

### Code Debt
- reading code is hard
- we spend more time reading code than writing code so anything we can do to write readable code is a really good investment
- confusing code slows you down
- examples of bad and complicated code:
  - large classes with thousands of lines
  - long methods with hundreds of lines
  - convoluted logic (hard to read IF statements)
  - cryptic names
  - worthless comments (i.e. ‘delete a customer’ comment on a ‘deleteCustomer’ method)
  - code smells (cut and paste)
- cut-and-paste code leads to repeated effort and divergent copies with different bug fixes and necessary tests
- tight coupling leads to complicated code, hard to reuse, hard to test, fragile and inflexible
- warning signs:
  - searching a lot for the line of code that needs changing
  - ending up having to modify a lot of files for a small change
  - a change to feature A ended up breaking feature B
    
### Architectural Debt
- flexibility matters, avoid making irreversible decisions and accept that requirements change
- examples:
  - missing or mixing layers (presentation, business logic, service, data access) which will lead to spaghetti code and untested code
  - missing extensibility points, similar features are a sign that such a point is needed to slot in easily (plugin architecture, abstraction, open closed principle)
  - overlooked concerns like passwords not encrypted, authentication but without authorization, no performance and scalability in mind
- warning signs:
  - too much effort to add new features
  - ‘too late to be implemented correctly’, existing debt breeding new debt

### Test Debt
- low unit test coverage of the business logic
- integration tests that are missing or not automated
- no real test data and some complex scenarios skipped
- no real world test environment (similar to production)
- unit testing makes refactoring easier as it is faster to rerun unit tests than manual tests
- no unit tests leads to little or no refactoring or even worse, refactoring without retesting
- warning signs:
  - same features keeps breaking again and again
  - a lot of bugs are found in production
  - developers afraid to modify code
    
### Knowledge Debt
- when no one knows what the code does, how does it work and who/what is using it
- the original author is no longer in the team/company and the documentation can be outdated
- the best example is dead code, features not used anymore but the code is still read by developers, can break the build via changes in dependencies, unit tests and wastes a lot of developer time
- warning signs:
  - time spend making obsolete code compile
  - keep reinventing the wheel (solution already in code but no one knows about it)
  - a lot of commits in history from devs no longer in the team or company
    
### Technological Debt
- there are 2 main challenges for this type of debt:
  - supporting new technologies, some of these changes are highly disruptive like new OS versions, 32 to 64 bit, web servers, browsers, etc.
  - migrating from legacy technologies, for example, Flash, Java Applets to HTML5, the devs need to know both
- warning signs:
  - trying to implement new industry standard features from scratch with existing framework
  - reliance on obsolete build tools, really hard to setup a local dev environment
  - telling customer they can’t update something (OS, browser) as it will break the app
    
## Quantifying technical debt
Various metrics can be used to objectively analyze the technical debt, reveal trends (getting better or worse), improve predictions but can be confusing, misleading (sometimes there’s no actual problem) and can be time-consuming.
Metrics should be automated, visible to everyone, meaningful, reliable (no false positives) and actionable (point to specific issues or locations).

### Types of metrics

#### 1. Time metrics
- increasing difference between estimates and actual time spent
- decreasing velocity strong indicator of technical debt

#### 2. Code metrics
Lines of code:
- simple to calculate
- poor measure of productivity and functionality
- can be a very good indicator of debt especially if parts that are growing rapidly are identified
- for example, classes with more than X methods, methods with more than Y lines
- identify ‘violations’ count and track this number over time, it should at least not increase
- do not rush to refactor violations, there are some exceptions like auto-generated code, simple repetitive code or code that is very stable

Cyclomatic complexity:
- characterized by high number of conditions, deeply nested logic and loops inside a method or code block
- complex methods are hard to maintain and test
- can generate false positives quite easily (simple switch with multiple cases, well-known algorithms)

Tips to identify and tackle code debt:
- a helpful code metric tool is Source Monitor which can also be used to keep track of metrics per version or checkpoint to help take decisions
- identify classes with a lot of modifications and look for way to refactor the code to make future modifications easier and quicker
- if a class/method has high complexity but is not used anymore do not waste time refactoring it just to lower the number of violations
- look for classes with all or most violations and a lot of changes recently

#### 3. Source control metrics
Code churn, which files/parts have been changed the most times.

Warning signs:
- lots of changes in the same file
- lots of authors for the same file
- classes that break the Single Responsibility Principle
- lack of extensibility (Open Closed Principle), hard to extend and have to modify existing code
- lots of bugs

False positives:
- auto-generated code
- global enumerations and lists

#### 4. Test metrics
- poor test coverage
- high number of manual tests that are impossible to do them all on each release, basically the ratio of executed tests vs all tests desired

Tips:
- keeping defect database (found & fixed dates, affected features, severity) can provide a lot of useful information like evolution of bugs per sprint, if they were found during QA or in production, how long it took to fix them (long time red flag)
- keeping a convention to link bug ticket ID in the commit message can help identify which files are regularly being bug fixed and low test coverage

### Communication
Collaboration is essential, both with other devs and management.

#### Devs
- devs can become cynical, critical, cautious (no refactoring), demoralized and frustrated
- some devs can be clueless about technical debt and need training (coding standards, unit testing, SOLID/DRY/YAGNI principles can improve things a lot)
- find others that are on the same page and gather momentum
- discuss or present technical debt by using real-world examples, maybe from the actual project but avoid the blame game

#### Management
- need to be aware of technical debt and greenfield vs brownfield development where you already have existing code that can have a big impact on future development
- one can try to estimate costs related to technical debt (based on time lost for example)
- it does not always make sense to refactor code (3 days for a bug fix vs 10 days for a full refactor) but once multiple bugs need to be fixed it tips the balance for the refactor, the source control metrics can help in this case
- the longer you have technical debt the more time it costs us and the harder it is to fix
- there are other business costs like time to market, reputation (because of bugs)
- we need to be pragmatic and strategic, not all of the debt can be repaid and most likely needs to be done in parallel with the normal development cycle.
- we need to make it visible:
  - when taking a shortcut for a bug fix, what problems it can cause and that we should repay it in the next version
  - when it is getting in the way, either repay now or postpone it with the associated risks but needs 2 estimates with long term ramifications clearly explained
  - stick to specific use cases which are easier to grasp and tackle instead of complaining in general

### Action plan
- strategically choose the most beneficial areas of technical debt to repay
- need to know the current state of the project, gaps in testing, complexity, code duplication so that the ‘pain points’ that cause problems for devs and fragility or rigidity for the project overall
- need to know the product roadmap because not all technical debt is equal and the areas that will be modified need to be prioritized
- need to have a clear vision for the future, what changes need to be done, coding style, patterns, practices and technologies with clear incremental steps
- create a technical debt document with the most important issues containing details about the problem, how it hinders development and what risks are introduced to the customer, what is the proposed solution, estimates, benefits
- review the document with technical debt before starting on a new feature/version allowing you to plan and prioritize

#### Examples of entries in a technical debt document
| Type  | Problem | Solution | Benefits | Estimate |
| ------------- | ------------- | ------------- | ------------- | ------------- |
| Complex code (XXX class) | - class too long<br> - too many responsibilities<br> - constantly needs to be bug-fixed<br> - little to no test coverage | - extract some responsibilities out<br> - use command design pattern<br> - increase unit test coverage | - easily add/remove functionality<br> - enables testability | 2 weeks |
| Architectural debt (lack of extensibility) | - support integration with customer stock DB requires changes in many places | - plugin interface | - rapid development of new integrations | 3 weeks |
| Architectural debt (inappropriate patterns) | - UserPreferences is a singleton<br> - tight coupling<br> - inability to override properties efficiently | - use DI and introduce a IoC container | - removes tight coupling<br> - allows easy overriding<br> - the solution can be used elsewhere | 2 weeks |
| Build process shortcomings | - automated tests not run<br> - devs forget to run them<br> - not all tests pass | - run as part of build and fail it if the tests fail | - highlight regressions immediately | 2 days |

### Practical techniques and strategies

#### 1. Cover it with tests and then modify it
- you can start with ‘characterization tests’ where you select representative inputs, exercise as many code paths as possible and examine the output
- in this case you assume the existing behaviour is correct (use common sense) and detect changes via assertions (change detectors)
- in case of tight coupling where unit tests are very hard to do, learn some techniques for making legacy code testable like ‘poor man’s dependency injection’, see ‘Working effectively with legacy code’ by Michael C. Feathers

#### 2. Make it extensible and then extend it
- extracting code blocks to methods, abstraction, etc.

#### 3. Modularise it and then rewrite it
- try to rewrite small portions, replacing them with extensible, maintainable and fully unit tested modules with appropriate design patterns
- tight coupling can get in the way and needs to be handled first by refactoring, for example, by using the facade pattern