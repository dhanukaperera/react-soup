# Enterprise UI Dev

If something failed? Who Did the code review.

Components of a Well-Architected UI

* Testing : Unit, component, integration, end-to-end, smoke tests, health checks.
* Static analysis : TypeScript, linting, formatting, etc.
* Build process : Automatically run checks for the items above.
* Separation of concerns: Keeping your business logic separate form the view layer.
* Deployment infrastructure: Do you even CDN?
* Design process: Are we thinking things out or are we throwing spaghetti at the wall?


**If you have to mock something, you are doing something wrong.**

Three Ingredients of a sustainable architecture?

* Patterns: Architecture,state management,abstractions.
* Processes: Code reviews, blueprints, design documents.
* Systems: Testing infrastructure, static analysis, build systems.

Testing tools

* Testing Library
* Vitest
* Playwright : Spinning a mini browser.

**It's hard to talk about maintainability without taking about testing.**


### The Spectrum of Testing

* Unit : Lighting fast, but tied to implementation
* Component Test
* End to End : Rest from yours perspective, but slow.

Integration Tests: Ideally, one or more units.

Component Tests : Render a single component from our application and pke at it.

Unit Testing : Your First Layer of Defense

A super simple Test

```javascript
import {expect, it} from 'vitest';

const add = (a: number, b: number) => a + b;

it('should correct add two numbers',()=>{
	expect(add(2, 4)).toBe(6);
})
```

Happy DOM : Virtual DOM in Node

`it` are `test` the same.


**Test passes when it does not fails.**

You can do the fail tests `it.fail()...`

vite is best when you use the react app and vite is faster. If you code base already using jest and webpack then no need to switch to vitest

Test can written in the following as .test or .spec or anything __test__

`npx vitest --ui` : open in a ui and view the tests

### Object Tests

describe blog orgnise a test, it's not manduarty.

toMatch Object
mock it
expect.any ex: expect id to any Number

File is test suite

`it.do` don't fail the tests

Common Test Pattern

* Setup
* Exercise
* Verify

You write a test what to expect and what to not to expect.

Do not repeat is not accpecible in writing test, you can repeat your code

Aysnc tests

* Add async await
* expect(returnValue).resolves.toBe(Value)

set time out sets are banners, toastes that wanis in a given time

