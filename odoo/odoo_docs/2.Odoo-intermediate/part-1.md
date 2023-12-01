# Odoo Development Process: Model-View-Controller (MVC) Architecture

## Overview

Odoo follows a Model-View-Controller (MVC)-like architecture. This documentation covers the following topics in a library project:

1. Creating a new addon module
2. Creating a new application
3. Adding automated tests
4. Implementing the model layer
5. Setting up access security
6. Implementing the backend view layer
7. Implementing the business logic layer
8. Implementing the website user interface (UI)

## Step 1 – Creating a New Addon Module

### Overview

- An addon module in Odoo is a directory containing files that implement features.
- It can add new features or modify existing ones.
- The directory must contain a __manifest__.py file (descriptor file).
- Some addon modules are featured as apps, appearing in the top-level Apps menu.

### Creating a Module

- Use the Odoo scaffold command: `(env15) $ odoo scaffold library_app ~/work15/library`.
- The command expects two arguments: the module directory name and the path for creation.
- For more details, run `odoo scaffold --help`.

### Module Directory Structure

- Example structure created by the scaffold command:
library_app/
├── init.py
├── manifest.py
├── controllers
│ ├── init.py
│ └── controllers.py
├── demo
│ └── demo.xml
├── models
│ ├── init.py
│ └── models.py
├── security
│ └── ir.model.access.csv
└── views
├── templates.xml
└── views.xml

- The technical name of the module must be a valid Python identifier.
- The directory contains several subdirectories for different components, following a widely used convention.

### The Manifest File

- The __manifest__.py file is a Python dictionary with attributes describing the module.
- Keys include 'name', 'summary', 'description', 'author', 'website', 'category', 'version', 'depends', 'data', and 'demo'.
- The manifest file asserts authorship, provides a basic description, and sets a distribution license.

## Step 2 to Step 8

(Not covered in the provided text, but presumably involves further steps in creating and managing the Odoo application.)

## Additional Topics

### Setting the Module Category

- Modules are grouped into categories representing functional areas.
- Categories are used for organizing addon modules and security groups.
- The XML ID for a category is generated from `base.module_category_` plus the category name.

### Adding an Icon

- Modules, especially those creating new apps, can have an icon.
- Add a `static/description/icon.png` file to the module for the icon.

### Upgrading Modules

- Upgrading is an iterative process, involving reloading data files and updating the database schema.
- Python logic changes may require a server restart.
- Use the command `(env15)$ odoo -c ~/work15/library.conf -d library -u library_app` to restart and upgrade.
- In recent versions, `--dev=all` mode automates this process, applying data file changes instantly and reloading Python code.

This document outlines the steps and considerations involved in creating and managing an addon module in Odoo, following its MVC-like architecture.
