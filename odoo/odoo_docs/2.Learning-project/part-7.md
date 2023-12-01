# Extending Modules in Odoo

## Overview

Odoo's architecture allows for the addition of features without directly modifying the code of existing modules. This capability ensures clean, isolated feature extensions in separate code components.

### Key Concept: Inheritance Mechanisms

- **Purpose:** Inheritance mechanisms in Odoo act as modification layers on top of existing objects. 
- **Scope:** These modifications can be applied at various levels, including the model, view, and business logic levels.

### Approach to Extending Modules

- **Creating New Modules:** 
  - Instead of altering an existing module, the preferred approach is to create a new module.
  - This new module adds a layer on top of the existing one, incorporating the desired modifications.

### Advantages

- **Isolation:** Modifications are kept separate from the original module, reducing the risk of conflicts and maintaining cleaner code.
- **Upgrade Safety:** Keeps the core module's upgrade path clear, as the original module remains untouched.
- **Flexibility:** Allows for easy activation or deactivation of features by simply enabling or disabling the extending module.

This approach underlines one of Odoo's strengths in flexible module development and customization, facilitating easier maintenance and scalability of features.

# Three Types of Inheritance in Odoo

Odoo supports different inheritance mechanisms, each serving specific purposes in module development and extension.

## 1. Classical Inheritance (`_inherit`)

- **Description:**
  - Classical inheritance is used to **EXTEND** an existing model.
  - It allows adding new fields, methods, or altering existing ones in a model.
- **Usage:**
  - Defined by using `_inherit = 'existing.model.name'` in the model class.
  - It doesn't create a new database table but extends the existing model's table.
- **Example Use Case:**
  - Adding new fields or methods to the `res.partner` model.

## 2. Prototype Inheritance (`_inherits`)

- **Description:**
  - Prototype inheritance is a delegation type of inheritance.
  - It's used to **create a new model** that inherits all fields and methods from another model but has its own independent database table.
- **Usage:**
  - Defined with `_inherits = {'other.model.name': 'field_name'}`.
  - The new model has a "composition" or Many2one relationship with the existing model.
- **Example Use Case:**
  - Creating a new model `hr.employee` that includes all fields of `res.partner` but stores data in a separate table.

```python
class HREmployee(models.Model):
    _name = 'hr.employee'
    _inherits = {'res.partner': 'employee.id'}
    employee_id = fields.Many2one(
    'res.partner',
    ondelete='cascade')
```
- In res.partner table will create a column named **employee.id**
- In hr.employee table will also create a column named **employee.id** and will linked to **employee.id** at res.partner table

## 3. Extension Inheritance (Inheriting Views)

- **Description:**
  - This type is used to **modify or extend VIEWS** of an existing model.
  - It allows **altering the model's form, tree, search views**, etc., without modifying the model's core structure.
- **Usage:**
  - Achieved by creating a new view that inherits from the existing view and making changes within it.
  - Utilizes the `inherit_id` field to reference the original view.
- **Example Use Case:**
  - Adding new fields to the form view of `product.template` or modifying the layout of the product form view.

Each inheritance type in Odoo serves a specific purpose, offering flexibility in how models and views can be extended or customized to fit business requirements.

### Example of View Inheritance

```xml
<odoo>
    <data>
        <record id="view_student_form_inherit" model="ir.ui.view">
            <field name="name">portal.student.form.inherit.student_course</field>
            <field name="model">portal.student</field>
            <field name="inherit_id" ref="portal_student_management.student_form" />
            <field name="arch" type="xml">
                <xpath expr="//form/sheet/group" position="after">
                    <group string="Courses">
                        <field name="course_ids"
                            options="{'no_create': True}">
                            <tree>
                                <field name="course_name" />
                                <field name="course_code" />
                            </tree>
                        </field>
                    </group>
                </xpath>
            </field>
        </record>
    </data>
</odoo>
```

This XML snippet demonstrates how to extend an existing form view in Odoo using view inheritance.

#### Record Definition

- **Model:** `ir.ui.view` 
  - The record is defined for a view, which is a common practice in Odoo to customize or extend user interfaces.

#### Fields in the Record

- **name:**
  - `portal.student.form.inherit.student_course` 
  - A unique name for the extended view.
- **model:**
  - `portal.student` 
  - Specifies the model this view is associated with.
- **inherit_id:**
  - `ref="portal_student_management.student_form"` 
  - References the original view that is being extended. This establishes the inheritance link.
- **arch:**
  - Contains the XML structure defining the view extension.

#### View Extension using XPath

- **XPath Expression:**
  - `//form/sheet/group` 
  - This targets a group element within a form view.
  - If an XPath expression matches multiple elements, only
the first one will be selected as the target for an
extension. Therefore, they should be made as specific as
possible 
- **Position Attribute:**
  - `after` 
  - The new group will be added after the targeted group element in the form.
- **New Group:**
  - Titled `"Courses"`
  - Contains a field `course_ids`, which shows a tree view of courses related to the `portal.student` model.
- **Fields Displayed:**
  - `course_name` and `course_code` 
  - Displayed within the tree view inside the `course_ids` field.

#### Purpose of This Extension

- This extension adds a new section to the existing `portal.student` form view, showing associated courses in a tree structure. 
- The use of `no_create: True` option in the `course_ids` field indicates that users cannot create new courses directly from this view, enhancing control over data entry.

# Extending Views in Odoo Using XML

## Overview

Views in Odoo are defined with XML and stored in the `arch` field. To extend a view, it's necessary to locate the appropriate XML node and perform the desired modifications, such as adding new XML elements.

### Simplified Notation for Extending XML

- **Matching Tags:** Use the XML tag to match, such as `<field>`, with distinctive attributes like `name` for precise targeting.
- **Position Attribute:** Declare the type of modification using the `position` attribute.

### Node Selection

- **Criteria:** Any XML element and attribute can be used to select the node, except for string attributes. This is because string attributes are translated based on the user's active language, making them unreliable selectors.
- **Extension Operations:** The operation to perform is declared with the `position` attribute. The available operations include:

#### Operations

1. **inside (default):**
   - Appends content inside the selected node, suitable for containers like `<group>` or `<page>`.

2. **after:**
   - Adds content after the selected node.

3. **before:**
   - Adds content before the selected node.

4. **replace:**
   - Replaces the selected node. Empty content implies deletion.
   - From Odoo 10, it can wrap an element with `$0` representing the element being replaced.
   - Example: `<field name="name" position="replace"><h1>$0</h1></field>`.

5. **attributes:**
   - Modifies attribute values of the matched element.
   - The content should contain `<attribute name="attrname">value</attribute>` elements.
   - An empty attribute tag removes the attribute from the element.

### Tip: Avoiding Deletion with `replace`

- **Recommendation:** Avoid using `position="replace"` for deletion, as it can break other modules relying on the node for extensions. Instead, make the element invisible to maintain compatibility.
