# Implementing the Model Layer in Odoo

## Overview

In Odoo, models are used to describe and store business object data, including a list of fields and attached business logic.

### Key Concepts

- **Model Data Structure:** Defined using Python code with classes derived from an Odoo template class.
- **Database Mapping:** Each model maps to a database table; Odoo handles database interactions and keeps the database structure in sync.
- **ORM Component:** The Object-Relational Mapping (ORM) component handles translation of transactions to database instructions.

## Example of a Model Definition

```python
from odoo import fields, models

class Book(models.Model):
    _name = "library.book"
    _description = "Book"
    name = fields.Char("Title", required=True)
    isbn = fields.Char("ISBN")
    active = fields.Boolean("Active?", default=True)
    date_published = fields.Date()
    image = fields.Binary("Cover")
    publisher_id = fields.Many2one("res.partner", string="Publisher")
    author_ids = fields.Many2many("res.partner", string="Authors")
```

## Explanation of Odoo Model Implementation

### Imports

- `models` and `fields` are imported from Odoo core objects.

### Model Class

- **Book:** A class derived from `models.Model`.

### Attributes

- **_name:** The unique ID (UID) for the model, used in Odoo.
- **_description:** A display name for the model records, used in user messages.

### Fields

- Various field types are used, including `Char`, `Boolean`, `Date`, `Binary`, `Many2one`, and `Many2many`.

### Tips and Conventions

- **Model IDs:** Use dot-separated words (`.`). Other elements like module names, XML IDs, and table names use underscores (`_`).

### Special Fields

- **name:** Used for the record display name.
- **active:** Filters out inactive records from the UI.
- **publisher_id:** A many-to-one relationship field.
- **author_ids:** A many-to-many relationship field.

### Special Fields Automatically Added by Odoo

- **id:** Unique numeric database ID for each record.
- **create_date** and **create_uid:** Record creation timestamp and the creating user.
- **display_name:** A textual representation for the record, often using the `name` field.
- **write_date** and **write_uid:** Last modification timestamp and the updating user.
- **__last_update:** A computed field for concurrency checks, not stored in the database.
