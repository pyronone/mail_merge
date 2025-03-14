# mail_merge


<!-- ... -->

At TELUS Health / LifeWorks, I have successfully executed over a dozen
mail merges, many of which had thousands of recipients, a wide variety
of templates, tight deadlines, and constant changes to both templates
and data.

With a natural inclination for process efficiency, I used Python scripts
to streamline the process. For the sake of organization,
maintainability, and documentation, I have bundled commonly used
functions from those scripts into a single package.

I have also tested, with great results, using
[docxtpl](https://docxtpl.readthedocs.io/en/latest/) for the actual
merge itself as the built-in MS Word function proved inconsistent and
unreliable, especially for larger merges and complex templates.

This package allows for effortless running and re-running of merges with
manual steps kept to a minimum. This is particularly useful when there
are frequent changes to the templates and/or data, and when there are
many different groups requiring different combinations of templates.

[Documentation](https://pyronone.github.io/mail_merge/)
