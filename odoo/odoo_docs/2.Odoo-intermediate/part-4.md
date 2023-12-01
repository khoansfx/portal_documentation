# Adding Access Control and Row-Level Access Rules in Odoo

## Access Control Security

To define access rules for a model in Odoo:

1. **Navigation:** Go to `Settings | Technical | Security | Access Rights` in the web client.
2. **Access Rights (ACL):** Define what actions (read, write, create, delete) are allowed on records for a security group.
3. **Data File:** These access rights are specified in a module data file, which loads records into the `ir.model.access` model. The filename for CSV data files should match the model ID.

### Example: Access Control in `course_management` Module

Add the file `security/ir.model.access.csv` with the content:

```csv
id,name,model_id:id,group_id:id,perm_read,perm_write,perm_create,perm_unlink
access_course_management_admin,course_management_admin,model_course_management,group_course_management_admin,1,1,1,1
access_course_management_user,course_management_user,model_course_management,group_course_management_user,1,0,0,0
```

## CSV File Columns for Access Control

- **id:** Record's external ID (XML ID), unique within the module.
- **name:** Descriptive title, recommended to be unique.
- **model_id:** External ID for the model, e.g., `model_library_book`.
- **group_id:** Identifies the security group for permissions.
- **perm_...:** Access permissions (1 for yes/true, 0 for no/false).

## Row-Level Access Rules

These rules define filters limiting the records a security group can access.

### Record Rules in Odoo

#### Location

- Found in the `Technical` menu, alongside `Access Rights`.

#### Storage

- Stored in the `ir.rule` model.

#### Definition Fields

- **name:** Distinctive, preferably unique, title.
- **model_id:** Reference to the model the rule applies to.
- **groups:** Reference to the security group the rule applies to. If not set, it's a global rule.
- **domain_force:** Domain filter for access restriction.

### Example: Adding a Record Rule to `course_management`

In the `security/course_security.xml` file, add a rule where `domain_force` is set to limit access based on certain conditions (e.g., course creator not being Administrator).

```xml
<!-- XML content for adding a record rule -->
<odoo>
    <data noupdate="0">
        <record id="module_category_course_management" model="ir.module.category">
            <field name="name">Course Management</field>
            <field name="sequence">1</field>
        </record>

        <!-- Admin Group -->
        <record id='group_course_management_admin' model='res.groups'>
            <field name="name">Admin</field>
            <field name="category_id" ref='module_category_course_management'></field>
            <field name="implied_ids" eval="[(4,ref('base.group_user'))]"></field>
        </record>

        <!-- User Group -->
        <record id='group_course_management_user' model='res.groups'>
            <field name="name">User</field>
            <field name="category_id" ref='module_category_course_management'></field>
            <field name="implied_ids" eval="[(4,ref('base.group_user'))]"></field>
        </record>

        <!-- Record Rule -->
        <record id="course_management_rule" model="ir.rule">
            <field name="name">Course Management Rule</field>
            <field name="model_id" ref="model_course_management"></field>
            <field name="groups" eval="[(4, ref('group_course_management_user'))]"></field>
            <field name="domain_force">[('course_creator', '!=', 'Administrator')]</field>
        </record>
    </data>
</odoo>

```

## Explanation of the Record Rule Addition

### Record Element

- **Code:** `<record id="course_management_rule" model="ir.rule">`
- **Description:** Adds a new record rule to the `ir.rule` model.
- **ID Attribute:** Uniquely identifies this rule as `"course_management_rule"`.

### Fields

- **Name Field:**
  - **Code:** `<field name="name">Course Management Rule</field>`
  - **Description:** Sets the name of the rule.
- **Model ID Field:**
  - **Code:** `<field name="model_id" ref="model_course_management"></field>`
  - **Description:** Links the rule to the `course_management` model.
- **Groups Field:**
  - **Code:** `<field name="groups" eval="[(4, ref('group_course_management_user'))]"></field>`
  - **Description:** Applies this rule to the `group_course_management_user` security group.
- **Domain Force Field:**
  - **Code:** `<field name="domain_force">[('course_creator', '!=', 'Administrator')]</field>`
  - **Description:** Sets the domain force condition, limiting access based on the `course_creator` field not being `"Administrator"`.


This record rule restricts the visibility of certain records to the users in the specified security group based on the defined conditions.
## Understanding `noupdate="1"` in Odoo XML Data Files

### Usage and Impact

- **Element:** `<data noupdate="1">`
- **Behavior:** Records within this element are created on module installation but not rewritten on module updates.
- **Purpose:** Allows for the rules to be customized post-installation without being overwritten by module upgrades.

## Detailed Explanation of `noupdate="1"` vs. `noupdate="0"` in Odoo

Odoo allows module developers to control how data records in XML files are treated during module updates or installations through the `noupdate` attribute.

### noupdate="1"

- **Behavior:**
  - When `noupdate="1"` is set in a `<data>` tag, the records inside this block are only created when the module is installed for the first time.
  - Subsequent updates to the module do not modify these records.
- **Use Case:**
  - Ideal for setting up initial data that should not be overwritten during module updates. This is particularly useful for records that might be customized directly in the production environment after installation.
- **Example Scenario:**
  - Access rights or configuration settings that are likely to be tailored to specific user needs post-installation.

### noupdate="0"

- **Behavior:**
  - When `noupdate="0"` is set (or the attribute is omitted, as `noupdate="0"` is the default behavior), the records inside this block are updated every time the module is updated.
- **Use Case:**
  - Suitable for data that must consistently reflect the module's code or structure, ensuring that updates in the module are propagated to these records.
- **Example Scenario:**
  - Structural data like model definitions, fields, views, or workflows that should remain consistent with the module's code.

### Tips for Development

1. **Using noupdate="0" During Development:**
   - While developing, using `noupdate="0"` ensures that any changes you make to the data records are applied when you update the module.
   - This facilitates testing and iterating on data changes.

2. **Switching to noupdate="1" Post-Development:**
   - Once development is complete and the module is ready for production or release, switch records that should not be auto-updated to `noupdate="1"`.
   - This prevents custom settings from being overwritten during module updates.

3. **Reinstalling Modules:**
   - If you need to force an update to records marked with `noupdate="1"`, one approach is to reinstall the module.
   - This can be done via the command line, using `-i` instead of `-u`.

### Conclusion

Understanding the use of `noupdate` in Odoo is crucial for effective module development and maintenance, ensuring that the right balance is struck between preserving customizations and applying necessary updates.
