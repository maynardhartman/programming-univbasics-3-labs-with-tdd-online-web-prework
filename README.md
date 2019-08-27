learn# Test Driven Development with RSpec

## Learning Goals

- Explore the RSpec testing environment
- Read and interpret tests
- Practice declaring and using variables

## Introduction

We've covered a lot of the basics of Ruby, but before we go further, its
important that we talk a bit more about _testing_. As part of this course, you
will encounter many lessons with tests that must be passed to register the
lesson as complete. These are referred to as _labs_. You've already completed a
few of them! All labs follow a similar format:

- Work in the provided files, testing out potential solutions. In this lab, for
  instance, code will need to be written in `calculator.rb` to pass the tests
- Run `learn` to print the tests at any point while you're writing your code
- Read the error messages produced by running the tests
- Write code that will resolve these error messages
- Run `learn` again to check your progress
- Repeat until all tests are passing
- Run `learn submit` to submit your solution

As the lesson material becomes more complex, so do the tests. Getting
acquainted with reading and interpreting tests will help you overcome some of
the toughest labs ahead. In this lesson, we're going to walk through reading
tests while also getting a little more practice with variables.

## Building a Simple Calculator

Your task in this lab is to build a simple calculator. Using variables, this
calculator will be able to take any two numbers and produce the result of their
addition, subtraction, multiplication and division. The exact specifications we
need to create this calculator are available to us... _in the tests_.

## Test Driven Behaviors

When we want to run an experiment, we need to develop a hypothesis and we need
to test it. In programming, we run tests to verify that programs behave the way
we think they do. Tests help us identify bugs and judge how healthy our
applications are.

We use tests to describe the program's behavior, just as you would in a
professional coding environment, and we also use them as teaching tools. You are
in charge of getting the tests to pass.

In Ruby, tests are handled by a tool called [RSpec][rspec]. RSpec is written in Ruby, but as
we will see, has some custom functionality built in specifically for writing and
running tests.

### Directory Structure

The structure of this lab — where its files and folders are located — looks
roughly like the following:

```text
├── CONTRIBUTING.md
├── LICENSE.md
├── README.md
├── calculator.rb
└── spec
    └── calculator_spec.rb
    └── spec_helper.rb
```

All labs will more or less have the same structure. (And non-lab lessons, for
that matter, will still have CONTRIBUTING.md, LICENSE.md, and README.md files.)
In Ruby, all labs will have a `spec` folder that contains our tests.

## Code Along

Open up `calculator.rb` in your text editor. If you're using the Learn IDE,
click the blue "Open IDE" button in the top right hand corner of the lesson. If
you open up that
`intro-to-ruby-programming-basics-test-driven-development-with-rspec/`
directory, you'll see a list of files (along with a `spec/` directory). Click
`calculator.rb`, and it will open in the editor.

In `calculator.rb`, you should see, well, nothing. We'll fix that soon.

Now open up `spec/calculator_spec.rb`. Hey, there's something! What's all of
this stuff doing?

**Note:** The `spec/calculator_spec.rb` has great info that we want to look at,
but **do not edit this file**. Otherwise, you may have extra difficulty passing
this lab.

A few lines down in the `spec/calculator_spec.rb` file you will see:

```ruby
describe "./calculator.rb" do
  # Lots of code written inside here
end
```

`describe` is a method provided by our test library, RSpec. We'll go into depth
on methods soon, but briefly: methods let us group up a set of statements into a
sort of bundle. Whenever that bundle's name is called, all the statements inside
are called in order. This is incredibly useful when we need to run the same
statements over and over (as we do running and rerunning tests).

The `describe` method holds our tests. Just after `describe` is a string,
`"./calculator.rb"`. Here, RSpec is telling us that the tests that come
afterwords will be about the file `calculator.rb`. We see the first one inside
another RSpec method, `it`:

```ruby
it "contains a local variable called first_number that is assigned to a number" do
  # code for first test is in here
end
```

The `it` method has a longer string describing what this test is for. In this
case, since this is _within_ the `describe` method, we can infer that
`calculator.rb` contains a local variable called `first_number`, and that the
variable is assigned to a number.

Looking at what is inside, we can see the actual test that has been described:

```ruby
it "contains a local variable called first_number that is assigned to a number" do
  first_number = get_variable_from_file('./calculator.rb', "first_number")

  expect(first_number).to be_an(Integer).or be_a(Float)
end
```

Reading the first line, we see a variable, `first_number` being assigned to
something, `get_variable_from_file`. This is actually another method! For now,
we don't really need to know what this method is doing (although we could
probably guess by its name). All we need to know is that the _result_ of
`get_variable_from_file('./calculator.rb', "first_number")` is getting assigned
to `first_number`.

Two lines down, we see something else that is new: `expect`.

```ruby
expect(first_number).to be_an(Integer).or be_a(Float)
```

This is the _actual_ test that will produce a passing or failing response when
we run `learn`. Read outloud, this line sounds like a normal English sentence:
_Expect first_number to be an integer or be a float_.

- `expect` is another RSpec method, indicating a test statement
- `first_number` is the variable defined two lines prior. It is the subject of
  the test
- `to` allows the test to define a positive expectation. Using `not_to` here
  would define a negative expectation
- `be_an` / `be_a` are known as [RSpec matchers][matchers]. In this case, they
  are for setting up the expectation that something is a certain data type
- `or` allows for two possible passing scenarios here: either `first_number` is
  an integer, or `first_number` is a float

If we run `learn` this test appears first, along with the string descriptions
we saw in `describe` and `it`:

```text
./calculator.rb
  contains a local variable called first_number that is assigned to a number (FAILED - 1)

Failures:

  1) ./calculator.rb contains a local variable called first_number that is assigned to a number
     Failure/Error: raise NameError, "local variable #{variable} not defined in #{file}."

     NameError:
       local variable first_number not defined in ./calculator.rb.
     # ./spec/spec_helper.rb:14:in `rescue in get_variable_from_file'
     # ./spec/spec_helper.rb:11:in `get_variable_from_file'
     # ./spec/calculator_spec.rb:6:in `block (2 levels) in <top (required)>'
     # ------------------
     # --- Caused by: ---
     # NameError:
     #   local variable `first_number' is not defined for #<Binding:0x00007fb7db153ca0>
     #   ./spec/spec_helper.rb:12:in `local_variable_get'
```

What if we were to create a variable called `first_number` inside
`calculator.rb`, but assigned it to something _other_ than a number? Say, for
instance, we wrote `first_number = "Hello world!"` in `calculator.rb`. Running
`learn` again, we would see this:

```text
./calculator.rb
  contains a local variable called first_number that is assigned to a number (FAILED - 1)

Failures:

  1) ./calculator.rb contains a local variable called first_number that is assigned to a number
     Failure/Error: expect(first_number).to be_an(Integer).or be_a(Float)

          expected "Hello world!" to be a kind of Integer

       ...or:

          expected "Hello world!" to be a kind of Float
     # ./spec/calculator_spec.rb:8:in `block (2 levels) in <top (required)>'
```

Notice that the error message has changed. The test was able to get the
variable `first_number` that we defined inside `calculator.rb`. It then runs
the test on `first_number` to see if it is a kind of integer or float. It isn't
either, so the test fails.

That's a lot to take in. Don't worry too much yet if it's hard to understand
what is happening inside of the `spec/calculator_spec.rb` file. But it's a good
idea to open up the file, and gather the information that you can, especially
when you are stuck on a lab. Reading test files and their results can give
targeted insight into what is breaking and where when you're writing a solution.

We will also provide instructions in the `README.md` file that will allow you to
complete the lab. However, the better you understand the tools you are using,
the better equipped you will be as we progress to more and more complex topics.

### Solving the Tests for this Lab

Now that you've got a sense of the tests in this lab, it is time to solve them
all. There are six in total. In this lab, testing is configured so that you will
only see the first test that fails along with any passing tests before that.
This means, as you pass each test, you'll see a growing list of passing tests
until you've passed them all. Run `learn` as you work through each step below
to see the test results.

- The first test we've started to solve already. The test is looking for a
  variable in `calculator.rb`, `first_number`. This variable should be set to
  an integer or float

- The second test is similar, but this time, looking for `second_number`.
  However, there is a second test here that must also pass:

```ruby
expect(second_number).not_to equal(0)
```

- The third test is looking for a local variable named `sum`. The `sum` variable
  is the result of adding `first_number` and `second_number` together. This test
  is using all three variables. Not only that, the test is using whatever values
  _you_ assigned to `first_number` and `second_number`.

- The fourth, fifth and sixth tests are similar to the tests for `sum`. Create
  the variable `difference` for subtracting, `product` for multiplying, and
  `quotient` for dividing the `first_number` and `second_number` variables.

**Hint:** If you're stuck on a particular variable, try writing the variable
in `calculator.rb` and assigning it a value you _know_ is incorrect. Tests may
produce different bits of useful information based on what you've written.

Once you have all tests passing, run `learn submit` to submit your solution.

## Conclusion

Labs are a major part of this course, and all labs rely on tests that you must
pass to register completion of the lesson. Being able to read and interpret
tests will help you unravel complex challenges ahead. More than that, though,
testing is a powerful tool to become familiar with.

[Test driven development][tdd] is a common process for developing programs in
which tests are written before code. A feature is first designed. Those designs
are then setup as test expectations. Once the tests are created, the actual code
is written to pass those tests.

Writing our own tests is still a bit further down the path of learning Ruby, but
being able to read tests is a skill that will be immediately helpful to you.

## Resources

[Test Driven Development][tdd]
[RSpec][rspec]

[tdd]: https://en.wikipedia.org/wiki/Test-driven_development
[rspec]: http://rspec.info/
[matchers]: https://relishapp.com/rspec/rspec-expectations/docs/built-in-matchers


