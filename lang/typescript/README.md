# TypeScript

TypeScript is a superset of JavaScript (ECMAScript) which adds static
type-checking. It compiles, via the TypeScript Compiler `tsc`, down to
JavaScript.

## Why use TypeScript?

- Static typing.
- Wide support.
- TypeScript is self-documenting.
- Safe(r).

### Static typing

Static types are types that don't change. Define something as a `bool` and it's
always a `bool`. This means N function calls later we can still determine that
`foo` is a `bool` without having to worry about whether or not running string
concatenate on it will result in a error; we already know it will. Now imagine
much less trivial examples over large teams (3+ people) and codebases (~100+
total lines of code).

?> _Note_ a team of 3+ people is large because now there is possibility for
communication to _not_ include someone. With 2 people both are always part of
the conversation. Remember, while you may understand something someone else may
not, and indeed future you may not either.

?> _Note_ codebases of ~200 lines or more are defined as large as you, nor
anyone else, can [grok](https://en.wikipedia.org/wiki/Grok) such a large amount.

?> _Note_ some heretics complain about TypeScript's lack of _true_ static
typing
(simply because it's optional via linting and compiler configuration); this is
actually a feature since it allows incremental changes to a codebase be made
rather than "all at once, okay now you can continue working".

### Wide support

While following trends is not neccersarily a good thing, in this case a lot of
tooling, libraries, and jobs are transitioning to TypeScript from JavaScript due
to its other benefits; thus using and learning it enables you to keep up with
the ecosystem.

### Self-documenting

Self-documenting code describes how to use it without (much) prerequisite
knowledge. No more guessing what the type of an object param is, it's part of
the language syntax/inference itself.

### Safe(r)

TypeScript checks at compile time which reduces the need for certain unit test
coverage, as well as catching errors before they occur at runtime. A less broken
app equates to more happy customers, and developers who have to maintain it.
