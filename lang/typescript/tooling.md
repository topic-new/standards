# TypeScript -- Tooling

Quick explanations on tooling that should be used within any project using
TypeScript, or TypeScript-based projects (e.g., a React frontend).

> TODO: seperate these out so they are reuseable

## Stylelint

Lints (checks) CSS and CSS-like syntaxes for errors, best practice, and redudant
styling.

## Prettierx

Formats code according to definable rules.

?> _Note_ `prettierx` is similar to `prettier` but less opinionated in certain
places to allow for the use of the `standard` JavaScript style.

## ESLint

Lints (checks) JavaScript (and TypeScript) code for errors according to a set of
definable rules. What ESLint does when a rule is broken is also configurable:
show error; show warning; etc.

?> _Note_ ESLint can also fix code however using Prettier does this better.
Also, ESLint LSP imeplementations vary so using `eslint --fix` isn't _always_ as
portable.

## Yarn

Node package manager, but:

- Faster (executes fetches and other actions in parallel rather than serially).
- Safer (true lock files; workspaces).
- More feature-rich (but not bloat).

## Babel

Transpiles ECMA features which may not be implemented in all/some browsers into
a common basepoint which most browsers will be able to use and understand;
allowing developers to write with the latest feature-set and end-users to use
said features.

## Lefthook

Git hooks done better than everyone else, because:

- Runs in parallel.
- Doesn't require Node, Ruby, or Python runtimes to execute.
- Has fewer dependencies.
- [And a lot, lot more.](https://github.com/Arkweid/lefthook/wiki/Comparison-with-other-solutions)
