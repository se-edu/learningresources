<frontmatter>
  title: Libraries
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

# Libraries

Authors: Li Kai

As a beginner, there may be some hesistation in using libraries. The big advantage is the amount of benefit it brings to the team. Whether its the overhead of having to load the library, or having to learn a large library's syntax, not having to maintain more code makes the project more manageable, maintainable and that alone makes it worth its cost.

## Utility Libraries

The limitation to Javascript is its limited utility library. That is why libraries like [lodash](https://lodash.com/) is such a popular module. There are a lot of functions being provided, which would help a lot in making shorter, more coherent syntax. Lodash reduces the need to write and maintain your own `_.assign` or `_.range` and even provides powerful functions such as `_.merge` and `_.groupBy`.

## Testing Libraries

There are a large amount of Javascript testing libraries. The below is a non-exhaustive list.

- [AVA](https://github.com/avajs/ava)
- [Jasmine](https://jasmine.github.io/)
- [Jest](https://facebook.github.io/jest/)
- [Mocha](https://mochajs.org/)
- [QUnitJs](https://qunitjs.com)
- [Tape](https://github.com/substack/tape)

So which one do you choose? Among the above, I have had the opportunity of write in all of the above except Tape. 

The general advice is to go for Mocha if you are new, due to its [large community](http://stateofjs.com/2016/testing/) and therefore overall amount of help available. When you are familiar with Mocha and understand its pitfalls (speed and organisation), you can explore other libraries such as Jest.

Jest, due to its speed, support for React.js, support for asynchronous testing and helpful terminal outputs, seems to be the upcoming major player. The fact that it is being made by Facebook is icing to the cake.

The odd test framework in the list above is QUnit, which is a test framework that runs on the browser instead of node.js like the others. QUnit has been around longer. Unless you are running JQuery and need to test for browser or UI related bugs, it is no longer a conventional choice.

</div>
