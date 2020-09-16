# RSpec Test Guidelines

RSpec is a DSL for creating testable examples of how code should run. It first
expresses and scopes tests in an English-like syntax and then executes the
actual test code; this makes it log-friendly and read/write friendly.

Tests deal with permutations of _state_ about a _thing_. The test cycle is
therefore:

```
(1) You create a thing (object which can be tested).
(2) Test the thing against some narrow criterion.
(3) Alter the things state.
(4) Repeat 2 and 3 until all reasonble permutations have been tested.
```

## General rules

### Follow Ruby conventions

  - `#` to refer to instance methods.
  - `::` (more visible than `.`) to refer to class methods.

## Groups via `describe` and `context`

  - [Docs 01 - describe and
    context](https://relishapp.com/rspec/rspec-core/v/3-9/docs/example-groups/aliasing)
  - [Docs 02 - eagerly loaded
    example_group](https://rspec.info/documentation/3.9/rspec-core/)

Unless stated otherwise statements about `describe` also refer to `context` as
both are aliases for `example_group`. You can define your own aliases for
semantics as described (here PUT LINK).

A `describe` block creates an example group. You can nest blocks within a
top-level `describe` using more `describe` or `context` blocks. Note, only
`describe` is a valid top-level block.

Every `example_group` within a file is eagerly loaded by RSpec when it reads said file.

### Aliasing

  - [Docs 01 -
    alias_example_group_to](https://www.rubydoc.info/github/rspec/rspec-core/RSpec%2FCore%2FConfiguration:alias_example_group_to)
  - [Docs 02 - other useful
    aliases](https://www.rubydoc.info/github/rspec/rspec-core/RSpec/Core/Configuration#alias_example_to-instance_method)

You can alias more terms other than `describe` and `context` to `example_group`
by passing the alias as a symbol to `c.alias_example_group_to`. You can
optionally also provide a [tag](#tagging) for the new alias.

Here `:detail` creates a custom alias which can be used as `.detail`.

```ruby
RSpec.configure do |c|
  c.alias_example_group_to :detail, :detailed => true
end

RSpec.detail 'detail alias' do
  it 'can do some stuff' do
  end
end
```

## Examples via `it` and hooks

Examples go within groups and represent testable cases.

An `example` is evaluated within the context of an instance of the
`example_group` class to which the example belongs. An `example` is not
executed when the spec file is loaded; instead, RSpec waits until all spec
files have been loaded, at which point it can apply filtering, randomization
before running an `example`.

## Example focus object via `subject`

There are three ways a `subject` is defined:

  1. Implicitly.
  1. Explicitly.
  2. In a one-liner.

Subject is lazily evaluated meaning it is not computed until it is executed within an `example`. See banged subject (PUT LINK).

### Implicit `subject`

  - [Docs 01 - implicit
    subject](https://relishapp.com/rspec/rspec-core/v/3-9/docs/subject/implicitly-defined-subject)

If the first argument of an `example_group` is a class then an instance of that
class is exposed to each example in that example group via the `subject` method
(RSpec will create an instance of that class and assign it to the `subject`).
This is the default behaviour and forms the **implicit case** for `subject`
definition.

```ruby
RSpec.describe User do
  # use of `subject` will implicitly refer to the an object of the class User
end
```

#### Nesting

`subject` can be nested from `example_groups` to an `example` naturally when
the classes are the same.

<details>
  <summary>Example</summary>

```ruby
class User
  def do_things
    true
  end
end

RSpec.describe User do
  it 'is a meaningless test' do
    expect(subject.do_things).to be(true)
  end

  describe 'a nested example_group' do
    it 'can have the same meaningless test here too' do
      expect(subject.do_things).to be(true)
    end
  end
end
```

```
$ rspec ./rspec-nested-sameclass.rb
2 examples, 0 failures
```
</details>

If the classes involved in the nesting are different the innermost class takes
precedence.

<details>
  <summary>Example</summary>

```ruby
class User
  def do_things
    true
  end
end

class UserFalseThings < User
  def do_things
    false
  end
end

RSpec.describe User do
  # same as before
  it 'is a meaningless test' do
    expect(subject.do_things).to be(true)
  end

  # note we're now describing a new UserFalseThings
  describe UserFalseThings do
    it 'is a meaningless test but on a new class now' do
      # if you try `be(true)` it will fail
      expect(subject.do_things).to be(false)
    end
  end
end
```

```
$ rspec ./rspec-nested-diffclass.rb
2 examples, 0 failures
```
</details>


### Explicit `subject`

  - [Docs 01 - explicit
    subject](https://relishapp.com/rspec/rspec-core/v/3-9/docs/subject/explicit-subject)

Pass a block to the `subject` method while within an `example_group` to define
the subject returned by the `subject` method.

```ruby
RSpec.describe Array do
  subject { [1, 2, 3] }
  # `subject` now refers to our Array with values [1, 2, 3]
end
```

You can also **name the explicit subject** by passing a symbol before the block
which creates a helper method of the same name. This helper method is memoized
within the `example` it's scoped to.

?> **note** `subject` set explicitly with or without a name also sets subject
anonymously (in context: implicitly) as setting a subject by name also sets the
`subject` method too. So, **the last explicitly defined `subject` is also the
`subject` accessed implicitly**.

?> **note** `subject` itself is also memoised within each `example` independent
of whether or not it's named.

```ruby
RSpec.describe Array do
  subject(:my_subject) { [1, 2, 3] }
  # `subject` or `my_subject` now refers to our Array with values [1, 2, 3]
end
```

<details>
  <summary>More Examples</summary>

#####

```ruby

```

```
```

---

```ruby

```

</details>

### Banged `subject` and `let`

A banged `subject!` and `let!` evaluate eagerly instead of lazily. This means
you can set state before an `example` runs. Do so sparringly.

### `subject` vs `let`

[Under the hood `subject` calls `let`](https://github.com/rspec/rspec-core/blob/main/lib/rspec/core/memoized_helpers.rb#L418) with some key differences:

  1. `let` does not set the implicit `subject`.
  2. `let` does not allow expectations on the implicit `subject`.
  3. `let` sets instance variables on an `example_group` for use between each `example`.

<details>
  <summary>Example</summary>

?> **note** These examples are not to be examined for best-practise but to
  demonstrate points.

Consider the following two classes.

```ruby
class User
  def do_things
    true
  end
end

class UserFalseThings < User
  def do_things
    false
  end
end
```

For which we will write some tests which will all pass.

```ruby
RSpec.describe User do
  it 'is accessing the implicit subject as we expect' do
    expect(subject.do_things).to be(true)
  end

  describe 'enter a new example_group' do
    # to break, change `subject` to `let`
    subject(:named_subject) { UserFalseThings.new }

    it 'is now accessing the named subject implicitly' do
      expect(subject.do_things).to be(false)
    end

    it 'is now accessing the named subject explicitly' do
      expect(named_subject.do_things).to be(false)
    end
  end
end
```

```
$ rspec ./rspec-let-vs-subject.rb
3 examples, 0 failures
```

Now suppose we change a single line, calling `let` instead of `subject`. Our
tests will fail as `let` has not set the implicit subject, meaning
`expect(subject.do_things)` refers to the User class, not the UserFalseThings
class. `let` did not, and does not, change the implicit subject.
</details>



## Tagging

Tags allow you to run specific examples without specifying every single file
and line number you want.

TODO all the ways to tag, not important at the moment.
TODO example
`:detailed` creates a custom tag such than you can run only example groups with a specific tag, e.g. `rspec run_some_spec.rb --tag detailed`.
