# Understanding Window Actions and Security in Odoo

## Window Actions

- **Menu Structure:** Menus in Odoo are structured as a tree with parent/child relationships. The leaf menu items are linked to specific actions that define their behavior upon selection. 
- **Action Title:** The name of the action is used as the title of the presented view.
- **Types of Actions:**
    - **Window Actions:** Most commonly used, they are employed to present views in the web client.
    - **Report Actions:** Utilized for running reports.
    - **Server Actions:** Designed to define automated tasks.

## Built-In Access Control

- **User Permissions:** Odoo includes built-in access control mechanisms ensuring that a user can only use the features they have been granted access to.

## Security Groups

- **Access Based on Groups:** Access control in Odoo is group-based. A security group is assigned access privileges on models, influencing the availability of menu items to users in that group.
- **Fine-Grained Control:** Further control is achieved through specific access to menu items, views, fields, and data records via record rules.
- **Organization Around Apps:** Security groups are organized around applications. Typically, each app offers at least two groups:
    - **User Group:** For performing daily tasks.
    - **Manager Group:** For performing all configurations related to the app.
- **Group Inheritance:** Members of a group inherit permissions from their parent groups. This includes accumulated permissions from all inherited groups.
- **Basic Access Group:** The Internal User group serves as the basic access group. App security groups generally inherit from this group.

## Security Record Rules

- **Default Access:** By default, users with access to a model can access all of its records.
- **Restricting Record Access:** To restrict user access to specific records, record rules are used. These rules set domain filters on models, enforced during read or write operations.
- **Domain Expression Example:** `[('create_uid', '=', user.id)]` can be used to restrict access to records created by the user.
- **Accessing Record Rules:** These rules can be found in `Settings | Technical | Security | Record Rules` or via the `View Record Rules` option in the developer menu.

## Understanding the Superuser Account

- **Special Privileges:** Odoo includes an internal root-like superuser named `OdooBot`, created automatically with database ID 1 upon database creation.
- **Bypassing Security Controls:** This superuser has special privileges that allow it to bypass security controls, used for internal operations or actions that require ignoring security controls.
- **Caution:** The use of the superuser should be limited to absolutely necessary cases. Its ability to bypass access security can lead to data inconsistencies, particularly in a multi-company context, and should generally be avoided.
