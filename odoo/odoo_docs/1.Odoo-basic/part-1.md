# Odoo Application Structure

Odoo applications are structured into three distinct tiers:

1. **Data Tier**
2. **Logic Tier**
3. **Presentation Tier**

## Data Tier

- **Database Server:** PostgreSQL is the exclusive database server supported by Odoo. This is a specific design choice made for the system.
- **Object Relational Mapping (ORM):** Odoo relies on its ORM engine as the interface between the apps and the database. The ORM serves as the application programming interface (API) used by addon modules for data interaction.
- **ORM Models:** The Data tier is implemented using ORM models. It is recommended that low-level database access should be limited to this layer to ensure secure access control and maintain data consistency.

## Storage of Binary Files

- **Filestore:** Binary files, such as document attachments and images, are stored in the filesystem in a directory known as the filestore.

## Logic Tier

- **Responsibilities:** This tier is responsible for all interactions with the Data tier and is managed by the Odoo server.
- **CRUD Operations:** Basic Create, Read, Update, and Delete (CRUD) operations can be extended to implement specific business logic.
    - For instance, the `create()` and `write()` methods might be used for setting default values or automating certain processes.
    - Additional methods can be implemented to enforce validation rules or to automatically compute field values.

## Presentation Tier

- **Role:** The Presentation tier is tasked with presenting data and interacting with users. It is implemented by the client part of the software, which handles end-user interaction.
- **Remote Procedure Calls (RPCs):** The client software uses RPCs to communicate with the Odoo service, which runs the ORM engine and business logic.
- **Odoo Web Client:** Odoo provides a default web client, supporting features like login sessions, navigation menus, data lists, and forms.
- **Website Framework:** A website framework is available for creating a public frontend for external users. It offers CMS features for building static and dynamic web pages.
- **Controller Components and QWeb Templating:** The website framework utilizes controller components for presentation-specific logic and QWeb for page rendering. QWeb templates are XML documents containing HTML markup and specific QWeb tags for various operations.
- **Open Server API:** Odoo’s server API is open, allowing access to all server functions. The API used by the official web client is also available to any other application.
- **Client Implementations:** Other client implementations are possible and can be developed on various platforms or programming languages. This allows for the creation of desktop and smartphone applications that leverage the Odoo Data and Logic tiers.

## Backup Requirements

- **Full Backup:** To create a full backup of an Odoo instance, it is necessary to have both:
  - A database dump
  - A copy of the filestore

[Part 1]
Part 2