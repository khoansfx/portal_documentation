# Creating a New Database in Odoo from the Web Client

## Accessing Odoo for the First Time

- **Initial Access:** When accessing Odoo for the first time and no databases are available, an assistant for creating a new database will appear.
- **URL:** Odoo is typically accessible at `http://localhost:8069` under the default configuration.

## Database Creation Form

### Required Information:

1. **Master Password**
   - It is the database manager password, stored in the Odoo configuration file. Recent versions of Odoo generate one automatically.
2. **Database Name**
   - This is the unique identifier name for the database. A single database server can host several Odoo databases with different names.
3. **Email**
   - Used as the login username for the default administrator user. It doesn't necessarily need to be an actual email address. The default value is `admin`.
4. **Password**
   - The secret password for administrator login.
5. **Language**
   - Default language for the database.
6. **Country**
   - Country setting for the company, relevant for localization in some apps like Invoicing and Accounting.
7. **Demo Data**
   - Enables the installation of demonstration data, desirable in development and test environments.

### Additional Notes:

- **Master Password Field:** Might be requested if set in the Odoo server configuration, preventing unauthorized administrative tasks.
- **Admin vs. Master Passwords:**
  - **Admin Password:** For the admin default user login, accessing database settings and user management.
  - **Master Password:** Accesses database manager features like backup, restore, and duplication of Odoo databases.
- **Database Creation Process:** After clicking the 'Create database' button, the process takes a few minutes. Once completed, you'll be redirected to the login screen.

## Database Manager

- **Access from Login Screen:** Located at the bottom as 'Manage databases'.
- **Functionality:** Provides options to back up, duplicate, or delete databases, and create new ones.
- **Direct Access URL:** `http://localhost:8069/web/database/manager`.

### Security Considerations

- **Default Settings:** By default, the database manager is enabled and unprotected by a password.
- **Security Recommendations:**
  - **Setting a Strong Master Password:** This can be done in the Odoo configuration file with `admin_passwd = <yourcomplexpassword>`.
  - **Disabling Database Manager:** Add `list_db = False` to the configuration file.
- **Configuration Details:** See the section 'Configuring the Odoo Server Options' for more on configuration files.
