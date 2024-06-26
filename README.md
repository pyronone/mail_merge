# mail_merge


<!-- WARNING: THIS FILE WAS AUTOGENERATED! DO NOT EDIT! -->

In 2 years at TELUS Health / LifeWorks, I’ve probably done over a dozen
mail merges of varying sizes. With a natural proclivity for process
improvement and efficiency, I mainly used Python scripts to streamline
the data cleaning and joining part of the process.

For the sake of improved organization, maintainability, and
documentation, I have bundled commonly used functions from those scripts
into a single module. I also experimented with using
[docxtpl](https://docxtpl.readthedocs.io/en/latest/) for the actual
merge as the standard, built-in MS Word function proved
unreliable/inconsistent, especially for larger merges.

This package allows for effortless running and re-running of merges with
manual steps kept to a minimum. This is particularly useful when changes
to templates and/or data are required, and when there are different
groups requiring different templates. More features will be added over
time.

[Documentation](https://pyronone.github.io/mail_merge/)
