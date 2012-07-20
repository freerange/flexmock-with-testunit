## Demo of Flexmock/Test::Unit integration

### How it works

- `require "flexmock/test_unit"` [1].
- this in turn does a `require "flexmock/test_unit_integration"` [2].
- this in turn does a `requires "test/unit"`
- defines a `FlexMock::TestCase` module which doesn't appear to be used anywhere and appears to duplicate the code that appears in "flexmock/test_unit".
- defines a `FlexMock::TestUnitFrameworkAdapter` class which has a single `#assertion_failed_error` method which determines whether to raise a `MiniTest::Assertion` or a `Test::Unit::AssertionFailedError` c.f. `Mocha::Integration::MiniTest.translate`.
- sets the `Flexmock.framework_adapter` to an instance of `FlexMock::TestUnitFrameworkAdapter`. This appears to be used whenever Flexmock wants to raise an assertion error.
- back in "flexmock/test_unit"
- this includes a couple of modules (`FlexMock::ArgumentTypes` and `FlexMock::MockContainer`) directly into `Test::Unit::TestCase` making various methods available on an instance of `TestCase` c.f. the `Mocha::API` module.
- it also does an `alias_method_chain` manoeuvre to add `Flexmock::MockContainer#flexmock_teardown` behaviour directly to `Test::Unit::TestCase#teardown`. This is monkey-patching `Test::Unit`, albeit not on quite the scale of Mocha. Note that the `#flexmock_teardown` behaviour encompasses both `#flexmock_verify` and `#flexmock_close` c.f. `Mocha::API#mocha_verify`, `Mocha::API#mocha_teardown`.

[1] https://github.com/jimweirich/flexmock/blob/master/lib/flexmock/test_unit.rb
[2] https://github.com/jimweirich/flexmock/blob/master/lib/flexmock/test_unit_integration.rb