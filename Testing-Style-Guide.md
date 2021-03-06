Style:
- [Mockists] (http://martinfowler.com/articles/mocksArentStubs.html) as in [GOOS](http://www.growing-object-oriented-software.com/)
- Classicists when testing with interfaces we do not own

Follows the 3-tier testing pyramid:

1. End-to-End Tests
  - there are smoke tests to basically ensure everything is wired up correctly
  - do not verify against models, only screen elements
  - [RSpec Integrated tests](https://robots.thoughtbot.com/rspec-integration-tests-with-capybara)
2. Acceptance Tests
  - Cucumber
3. Unit Tests
  - RSpec

Support tools:
- [SimpleCov](https://github.com/colszowka/simplecov) to ensure 100% test coverage.
