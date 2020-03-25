---
title: Running Tests with pytest
teaching: 10
exercises: 0
questions:
- "How do I automate my tests?"
objectives:
- "Understand how to run a test suite using the pytest framework"
- "Understand how to read the output of a pytest test suite"
keypoints:
- "The `pytest` command collects and runs tests starting with `Test` or `test_`."
- "`.` means the test passed"
- "`F` means the test failed or erred"
- "`x` is a known failure"
- "`s` is a purposefully skipped test"
---

We created a suite of tests for our mean function, but it can be annoying to run
them one at a time. In particular, if one test fails and causes an exception, the next tests after that will not automatically run.
It would be a lot better if there were some way to run them all at once, just reporting which tests fail and which succeed.

Firstly, install the PyTest testing framework: `python -m pip install pytest`.

Thankfully, that exists. Recall our tests:

~~~
from mean import *

def test_ints():
    num_list = [1,2,3,4,5]
    obs = mean(num_list)
    exp = 3
    assert obs == exp

def test_zero():
    num_list=[0,2,4,6]
    obs = mean(num_list)
    exp = 3
    assert obs == exp

def test_double():
    num_list=[1,2,3,4]
    obs = mean(num_list)
    exp = 2.5
    assert obs == exp

def test_complex():
    # given that complex numbers are an unordered field
    # the arithmetic mean of complex numbers is meaningless
    num_list = [2 + 3j, 3 + 4j, -32 - 2j]
    obs = mean(num_list)
    exp = NotImplemented
    assert obs == exp
~~~
{: .python}

Once these tests are written in a file called `test_mean.py`, the command
`pytest` can be run on the terminal or command line from the directory containing the tests (`python -m pytest` also works, if you want to ensure pytest uses the correct Python executable):

~~~
$ pytest
~~~
{: .bash}
~~~
collected 4 items

test_mean.py ...F

================================== FAILURES ===================================
________________________________ test_complex _________________________________

    def test_complex():
        # given that complex numbers are an unordered field
        # the arithmetic mean of complex numbers is meaningless
        num_list = [2 + 3j, 3 + 4j, -32 - 2j]
        obs = mean(num_list)
        print(obs)
        exp = NotImplemented
>       assert obs == exp, "the arithmetic mean of complex number numbers is meaningless"
E       AssertionError: the arithmetic mean of complex number numbers is meaningless
E       assert (-9+1.6666666666666667j) == NotImplemented

test_mean.py:28: AssertionError
===================== 1 failed, 4 passed in 2.71 seconds ======================
~~~
{: .output}

In the above case, the pytest package 'sniffed-out' the tests in the
directory, by looking recursively looking for files named `[Tt]est_*.py` or `[Tt]est-*.py`, and then run the functions inside those files called `[Tt]est_*`.
It runs them together to produce a report of the sum of all executed files and functions.

The major benefit a testing framework provides is exactly that, a utility to find and run the
tests automatically. With pytest, this is the command-line tool called
`pytest`.  When `pytest` is run, it will search all directories below where it was called,
find all of the Python files in these directories whose names
start or end with `test`, import them, and run all of the functions and classes
whose names start with `test` or `Test`.
This automatic registration of test code saves tons of human time and allows us to
focus on what is important: writing more tests.

When you run `pytest`, it will print a dot (`.`) on the screen for every test
that passes,
an `F` for every test that fails or where there was an unexpected error.
In rarer situations you may also see an `s` indicating a
skipped tests (because the test is not applicable on your system) or a `x` for a known
failure (because the developers could not fix it promptly). After the dots, pytest
will print summary information.

Without changing the tests, alter the mean.py file from the previous section until it passes. 
> Hint
>
> A combination of the `any()` and `isinstance()` functions, and a list comprehension or generator expression may do the trick.
{: .callout}

When it passes, `pytest` will produce results like the following:

~~~
$ pytest
~~~
{: .shell}

~~~
collected 4 items

test_mean.py ....

========================== 4 passed in 1.14 seconds ===========================
~~~
{: .output}

> ## Show what tests are executed
>
> Using `pytest -v` will result in `pytest` listing which tests are executed
> and whether they pass or not:
> ~~~
> $ py.test
> ~~~
> {: .bash}
>
> ~~~
> collected 5 items
>
> test_mean.py .....
>
> test_mean.py::test_ints PASSED
> test_mean.py::test_zero PASSED
> test_mean.py::test_float PASSED
> test_mean.py::test_complex PASSED
>
> ========================== 5 passed in 2.57 seconds ===========================
> ~~~
> {: .output}
>
{: .callout}

As we write more code, we would write more tests, and pytest would produce
more dots.  Each passing test is a small, satisfying reward for having written
quality scientific software. Now that you know how to write tests, let's go
into what can go wrong.

> Output from functions when running pytest
>
> Pytest suppresses all output from your tested functions: it ignores stdout, and tests for the return values from functions. If you do want to see output during testing, use the `-s` option.
{: .callout}

> Running a specifc test
>
> During development, you may not want to run all tests all the time.
> Sometimes, you are just developing a new test, which is the only test you want to run.
> `pytest` takes a `-k` option with an argument that is a case-insenstive string, which is matched to the file or function name.
> For example, `pytest -k float` will only run the `test_float` test.
{: .callout}


{% include links.md %}
