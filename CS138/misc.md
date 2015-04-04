Misc. Notes
==========

## Testing
**Regression Testing** - Code first, test after.

**Test Driven Development (TDD)** - Create the test cases first, then code to pass those tests.

## Adapters and Design Principles
### Adapters / Wrappers
* Usually you have a data structure with what you want, but the API isn't exactly what you want.

Instead:

1. Design your API
2. Instantiate - **not inherit** - an object from the "workhorse" (data structure) class
3. Use the "workhorse" object's methods to achieve your goals.

By doing so, you ensure that the user only uses what you want them to use.

The STL includes adapters for `stack`, `queue`, and `priority_queue`, using the `vector`, `deque`, or `list`.