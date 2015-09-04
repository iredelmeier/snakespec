# SnakeSpec #

Simple behaviour-driven development for Python, inspired by RSpec and Jasmine.
Currently available as a plugin for [Nose](https://nose.readthedocs.org/en/latest/).

_SnakeSpec is currently in alpha mode. Use at your own risk!_  
_Feedback and contributions are much appreciated._

## Features ##

* Nested test cases that inherit setup, etc. from the enclosing class(es)
* Flexible naming of test cases and methods for a BDD-style flow

## Installation ##

For regular usage:

    pip install -i https://testpypi.python.org/pypi snakespec

From the source:

    python setup.py

## Usage ##

    nosetests --snakespec

or

    export NOSE_SNAKESPEC=true
    nosetests

## Writing Tests ##

* Decorate nested test cases with the `@snakespec.describe` decorator
* Name the nested, decorated test cases (classes) as you please, e.g., 'GivenSomeCircumstance'
* Test methods can be named using `[Ii]t|[Ss]hould|[Tt]hen`, in addition to `[Tt]est` and the configured
[testMatch](https://nose.readthedocs.org/en/latest/usage.html?highlight=testmatch#cmdoption--testmatch)


    from unittest import TestCase
    from snakespec.snakespec import describe
    
    class TestSomeClass(TestCase):
        def setUp(self):
            self.foo = 'foo'
            
        def it_should_run_normally(self):
            self.assertEqual(self.foo, 'foo')
            
        @describe
        class GivenSomeCircumstance(TestCase):
            def setUp(self):
                super(TestSomeClass.GivenSomeCircumstance, self).setUp()
                self.bar = 'bar'
                
            def it_should_inherit_setup(self):
                self.assertEqual(self.foo, 'foo')
                
            def it_should_use_its_own_setup(self):
                self.assertEqual(self.bar, 'bar')

            @describe
            class WhenSomethingHappens(TestCase):
                def setUp(self):
                    super(TestSomeClass.GivenSomeCircumstance.WhenSomethingHappens, self).setUp()
                    self.foobar = self.foo + self.bar
                    
                def it_should_inherit_setup_from_all_ancestors(self):
                    self.assertEqual(self.foobar, 'foobar')

## Planned Features ##

* Test coverage (oh, the irony!)
* Documentation on PyPI and Read the Docs
* Further examples
* Support for additional versions of Python, including Python 3
* Usage as a unittest2 plugin
* Usage as a pytest plugin

## Contributing ##

[Code](https://github.com/iredelmeier/snakespec/)  
[Issues and feature requests](https://github.com/iredelmeier/snakespec/issues)  
[Pull requests](https://github.com/iredelmeier/snakespec/pulls)
