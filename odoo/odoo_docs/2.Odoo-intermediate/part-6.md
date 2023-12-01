# Access Security in Odoo

## Internal System Models and Relevant Fields

### res.groups (Groups)

- **Relevant Fields:** 
  - `name`
  - `implied_ids`
  - `users`

### res.users (Users)

- **Relevant Fields:** 
  - `name`
  - `groups_id`

### ir.model.access (Access Control)

- **Relevant Fields:** 
  - `name`
  - `model_id`
  - `group_id`
  - `perm_read`
  - `perm_write`
  - `perm_create`
  - `perm_unlink`

### ir.access.rule (Record Rules)

- **Relevant Fields:** 
  - `name`
  - `model_id`
  - `groups`
  - `domain_force`

## XML IDs for Security Groups

- **base.group_user:** Internal user, applicable to any backend user.
- **base.group_system:** Settings, includes the Administrator.
- **base.group_no_one:** Technical feature, often used to hide features from users.
- **base.group_public:** Public, for features accessible to web anonymous users.

## XML IDs for Default Users Provided by Odoo

- **base.user_root:** The root system superuser, also known as OdooBot.
- **base.user_admin:** The default user, typically named Administrator.
- **base.default_user:** Template for new backend users. Inactive, but can be duplicated for new user creation.
- **base.default_public_user:** Template for creating new portal users.
