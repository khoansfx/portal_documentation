# Step 2 – Creating a New Application in Odoo

## Overview

Creating a new application in Odoo involves adding unique features or modifying existing applications. Key characteristics of an app include an icon, a top-level menu item, and security groups.

### Adding a Top Menu Item

- **Purpose:** A main menu item is essential for a new app, appearing in the top-left drop-down menu on the Community Edition (CE).
- **Implementation:** Menu items are added using XML data files.

#### Example XML for Menu Item

```xml
<odoo>
    <!-- Library App Menu -->
    <menuitem id="menu_library" name="Library" />
</odoo>
```

## Description of the Menu Item XML Element

- **Menu Item Element:** The `<menuitem>` element writes a record on the `ir.ui.menu` model, where Odoo stores menu items.
- **XML ID:** This ID is used to uniquely identify each data element. Submenu items will reference their parent menu using this ID.
- **Manifest File:** The new XML file should be declared in the data attribute of the `__manifest__.py` file.

## Tips for Menu Item Visibility

- **Menu Tree Visibility:** Menu tree items appear only if there are visible submenu items.
- **Lower-Level Menu Item Visibility:** Lower-level menu items that open views are visible only if the user has access to the corresponding model.

## Adding Security Groups

### Purpose of Security Groups

- Security groups are essential for granting access to features in Odoo.

### Typical Access Levels in Odoo Apps

- Odoo apps usually provide two groups:
  - **User Access Level:** For users performing daily operations.
  - **Manager Access Level:** For users with full access to all features, including configurations.

### Location for Security Files

- Security-related files are often kept in a `security/` module subdirectory.

### Example XML for Security Groups

```xml

<odoo>
    <data noupdate="0">
        <record id="module_category_course_management" model="ir.module.category">
            <field name="name">Course Management</field>
            <field name="sequence">1</field>
        </record>
        <!-- Admin Group-->
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
    </data>
</odoo>
```
## Explanation of the Security Group XML

### Record for Module Category

- **Element:** `<record>` with `id="module_category_course_management"` and `model="ir.module.category"`.
- **Purpose:** Creates a new category in the system named "Course Management."

### Fields in the Record

- **name:** Represents the title of the group.
- **category_id:** Links to the category created earlier.
- **implied_ids:** 
  - Defines additional groups that users in this group will also belong to.
  - Utilizes the internal user XML ID `base.group_user`.

##### Odoo Security Group `implied_ids` Codes and Their Behaviors

The `implied_ids` field in Odoo's XML is used to define relationships between security groups. Below is a table summarizing the common codes and their behaviors:

| Code                                 | Behavior                                      | Usage                                           |
|--------------------------------------|-----------------------------------------------|-------------------------------------------------|
| `(4, ref('xml_id'))`                 | Adds a link to an existing group.             | Include users in the group identified by `xml_id`. |
| `(3, ref('xml_id'))`                 | Removes a link to the group.                  | Remove users from the group identified by `xml_id`. |
| `(6, 0, [ref('xml_id1'), ref('xml_id2')])` | Replaces links with specified groups.     | Set groups to the list provided, removing others.  |
| `(5, 0, 0)`                          | Removes all links (clears all implied groups).| Clear all implied groups from the defined group.   |

This table outlines the syntax and implications of different codes used in the `eval` attribute of the `implied_ids` field, allowing for the dynamic configuration of security groups in Odoo.


## Explanation of the Provided XML Configuration in Odoo

### Module Category

- **Record Element:**
  - `<record id="module_category_course_management" model="ir.module.category">`
    - Creates a new record in the `ir.module.category` model.
    - Unique identifier for this record is "module_category_course_management".

- **Fields:**
  - `<field name="name">Course Management</field>`:
    - Sets the name of the category to "Course Management".
  - `<field name="sequence">1</field>`:
    - Assigns a sequence number, influencing its ordering in lists.

### Admin Group

- **Record Element:**
  - `<record id='group_course_management_admin' model='res.groups'>`
    - Creates a new record in the `res.groups` model, defining a security group.
    - Unique identifier is "group_course_management_admin".

- **Fields:**
  - `<field name="name">Admin</field>`:
    - Names the group "Admin".
  - `<field name="category_id" ref='module_category_course_management'>`:
    - Associates the group with the module category "Course Management".
  - `<field name="implied_ids" eval="[(4,ref('base.group_user'))]">`:
    - Includes the base group "base.group_user" in this group.

### User Group

- **Record Element:**
  - `<record id='group_course_management_user' model='res.groups'>`
    - Similar to the Admin group, defines another security group in the `res.groups` model.
    - Unique identifier is "group_course_management_user".

- **Fields:**
  - `<field name="name">User</field>`:
    - Names the group "User".
  - `<field name="category_id" ref='module_category_course_management'>`:
    - Links this group to the same module category as the Admin group.
  - `<field name="implied_ids" eval="[(4,ref('base.group_user'))]">`:
    - Also includes the base group "base.group_user" in this group.

#### General Overview

This XML configuration is commonly used in Odoo for setting up a module. It establishes a new category named "Course Management" and organizes specific security groups (Admin and User) under this category. These groups are utilized for access control within the Odoo application, allowing for differentiated permissions and access levels for administrative and regular users.

