---
layout: post
title: Python coding style guide
date: 2016-03-31 11:30:00 +0800
author: Yuan Jiang
tags: python
---

PEP8 (Python Enhancement Proposal 8) is the style guide for python coding and this post lists some of the important rules that should be followed.

NOTE that for more details PEP8 should always be referenced and the following section itself is taken from the section of [Item 2: Follow the PEP 8 Style Guide](http://www.effectivepython.com/) of the book _Effective Python_.

## Whitespace
- Use spaces instead of tabs for indentation.
- Use four spaces for each level of syntactically significant indenting.
- Lines should be 79 characters in length or less.
- Continuations of long expressions onto additonal lines should be indented by four extra space from their normal indentation level.
- In a file, functions and classes should be separated by two blank lines.
- In a class, methods should be separated by one blank line.
- Don't put spaces around list indexes, function calls, or keyword argument assignments.
- Put one - and only one - space before and after variable assignments.

## Naming
- Functions, variables, and attributes should be in lowercase_underscore format.
- Protected instance attributes should be in _leading_underscore format.
- Private instance attributes should be in __double_leading_underscore format.
- Classes and exceptions should be in CapitalizedWord format.
- Module-level constants should be in ALL_CAPS format.
- Instance methods in classes should use self as the name of the first parameter (which refers to the object).
- Class methods should use cls as the name of the first parameter (which refers to the class).

## Expressions and Statements
- Use inline negation (if a is not b) instead of negation of positive expressions (if not a is b).
- Don't check for empty values (like [] or '') by checking the length (if len(somelist) == 0). Use if not somelist and assume empty values implicitly evaluate to False.
- The same thing goes for non-empty values (like [1] or 'hi'). The statement if somelist is implicitly True for non-empty values.
- Avoid single-line if statements, for and while loops, and except compound statements. Spread these over multiple lines for clarity.
- Always put import statement at the top of a file.
- Always use absolute names for modules when importing them, not names relative to the current module's own path. For example, to import the foo module from the bar package, you should do from bar import foo, not just import foo.
- If you must do relative imports, use the explicit syntax from . import foo.
- Imports should be in sections in the following order: standard library modules, third-party modules, your own modules. Each section should have imports in alphabetical order.

## References
- [PEP8](https://www.python.org/dev/peps/pep-0008/)
- [Pylint](https://www.pylint.org/)
- [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html)
- [The Hitchhiker's Guide to Python](http://docs.python-guide.org/en/latest/writing/style/)
