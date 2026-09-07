# Mail Merges, Modernized


<!-- ... -->

## Overview

This package provides a modern alternative to standard Word mail merge,
using Jinja2-like templating syntax to enable advanced features and
streamline document generation workflows.

This started with the reality of nationwide regulatory work: every
province has its own rules, which meant its own templates — and within
each, a matrix of entitlement types (locked-in, non-locked-in, cash
only, partial payments, immediate vs. deferred pensions) that multiplied
the template count fast.

Imagine this. Dozens of templates across provinces and entitlement
types. Templates get tweaked constantly, and actuarial numbers aren’t
always right the first time, which means reopening each document,
clicking through Word’s merge wizard, exporting to PDF, and renaming
every file by hand, over and over, under a tight deadline.

That’s hours of tedious, repetitive work — and it only gets worse as
templates multiply.

So I built a tool that turns all of that into a single command. Update a
template or fix a number, and the entire batch re-merges automatically —
no reopening files, no manual exporting, no renaming. What used to take
hours now takes minutes, and it scales just as easily to five templates
as it does to fifty.

## Why Use This Instead of Standard Mail Merge?

### Key Advantages

- **Powerful templating syntax**: Jinja2-like tags enable conditional
  logic, loops, and dynamic content
- **Rich formatting support**: Maintain text formatting, insert images,
  and create dynamic tables
- **Automatic field detection**: Quickly extract all required fields
  from templates without manual inspection
- **Scalability**: Efficiently manage large numbers of templates across
  multiple groups
- **Type safety**: Better validation and error handling compared to
  traditional merge fields

## Comparison: Standard Mail Merge vs. This Package

| Feature | Standard Mail Merge | This Package |
|----|----|----|
| Syntax | `« MERGEFIELD »` | `{ field_name }` |
| Conditional logic | Limited | Full support |
| Dynamic tables | Manual setup | Automatic with loops |
| Image insertion | Complex | Simple tag syntax |
| Field extraction | Manual inspection | Automatic detection |
| Rich text | Basic | Full formatting support |
| Scalability | Decreases with complexity | Handles large template sets easily |

## Template Workflow

### Step 1: Adding Tags to Your DOCX Templates

Unlike standard Word merge fields (which use `« MERGEFIELD »` syntax),
this package uses Jinja2-style tags that are more readable and powerful.

#### Basic Tag Syntax

    {{ field_name }}

Simply type these tags directly into your Word document where you want
dynamic content to appear.

#### Example Template

    Dear {{ recipient_name }},

    Thank you for your purchase of {{ product_name }} on {{ purchase_date }}.

    Your order total: {{ order_total }}

    Best regards,
    {{ sender_name }}

#### Conditional Content

Use if-statements to show or hide content based on your data:

    {% if premium_customer %}
    As a valued premium customer, you receive free shipping on all orders!
    {% endif %}

#### Dynamic Tables

Generate table rows dynamically from list data:

    | Item | Quantity | Price |
    |------|----------|-------|
    {%tr for item in items %}
    | {{ item.name }} | {{ item.quantity }} | {{ item.price }} |
    {%tr endfor %}

#### Rich Text Variables

Variables can contain inline formatting — bold, italic, underline,
color, font size — so styled content merges cleanly into your document
without splitting it across multiple fields.

    {{r richtext}}

#### Dynamic Images

Swap in different images at merge time based on your data — useful for
logos, signatures, charts, or any image that varies per recipient.

    {{ logo }}

### Step 2: Field Extraction

Automatically scan a template, or folder of templates (searches
recursively in sub-folders), and list every field it requires — no need
to track them manually.

This is especially handy when working with a large number of templates,
each with their own unique set of fields.

``` python
tags = getFieldTags(extractFromDocx(tpl_dir))
sorted(tags)
```

    ['{% endif %}',
     '{% if amt_cash %}',
     '{% if pct_rome %}',
     '{{ amt_cash }}',
     '{{ amt_li_ls }}',
     '{{ amt_partial_ls }}',
     '{{ dt_doe }}',
     '{{ dt_doh }}',
     '{{ dt_dot }}',
     '{{ dt_mdob }}',
     '{{ dt_nrd }}',
     '{{ dt_partial_ls }}',
     '{{ dt_send }}',
     '{{ img_logo }}',
     '{{ pct_rome }}',
     '{{ str_footer }}',
     '{{ str_greet }}',
     '{{ str_id }}',
     '{{ str_ls_msg }}',
     '{{ str_mailing_details }}',
     '{{ str_mbr_name }}',
     '{{ str_sex }}']

### Step 3: Running the Merge

Once your templates are tagged and your data is prepared, running the
merge is a single command:

``` python
merge("./path/to/config.yml")
```

- Pass `_test=n` to run only the first `n` records — useful for quickly
  checking formatting before a full run.
- Pass a list into `_test_ids` to check specific IDs (special cases,
  etc.)

#### Config file

The merge function takes a YAML config file as its primary parameter.
This keeps your merge setup readable and easy to edit without touching
code, serves as a clear record of where your data lives, how templates
map to recipient groups, and can be version-controlled.

Here is a simple example:

``` yaml
data: C:/path/to/data.parquet
id_col: id
group_col: _group
groups:
- example:
  - C:/path/to/template.docx
image_cols:
- img_logo
```

There is also a helper function (`helpers.initCfg`) to help generate
this config file automatically.
