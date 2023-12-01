# Managing Server Log Messages in Odoo

## Default Behavior

- **Log Output:** By default, Odoo prints server log messages to standard output, visible in the terminal window.

## Example Log Line

- **Structure of Log Line:** Log lines in Odoo follow a structured format. For example:  
  `2021-11-08 08:06:57,786 18592 INFO 15-demo odoo.modules.loading: Modules loaded.`

### Components of a Log Line

1. **Timestamp:** `2021-11-08 08:06:57,786` - Date and time of the log message, in UTC.
2. **PID:** `18592` - System process ID.
3. **Log Level:** `INFO` - Indicates the severity level of the message.
4. **Database Name:** `15-demo` - The database context. '?' is used for non-database-specific actions.
5. **Module:** `werkzeug` - Odoo module posting the message, e.g., `odoo.modules.loading` for module loading actions.
6. **Message Content:** Describes the log message, which can vary based on the action logged.

## HTTP Request Logs

- **Example:**  
  `2021-11-08 08:06:57,786 18592 INFO 15-demo werkzeug: 127.0.0.1 - - [08/Apr/2020 08:06:57] "POST /web/dataset/call_kw/res.partner/read HTTP/1.1" 200 - 213 0.135 0.092`
- **Details Included:** Source IP address, the endpoint called, HTTP status code, and performance information.

### Performance Numbers
`213 0.135 0.092`

1. **Query Count: 213** Number of SQL queries executed.
2. **SQL Query Time: 0.135** Time spent running SQL queries.
3. **Non-SQL Time: 0.092** Time spent on non-SQL activities (mostly Python code).

## Log Settings Control

- **Log Verbosity:** Controlled by the `--log-level` option. Default is `info` level.
- **Verbosity Levels:**
  - `warn`: Only warnings and errors.
  - `error`: Only errors.
  - `critical`: Only server-functionality preventing errors.
  - `debug`: Enables debug-level messages.
  - `debug_sql`: Displays executed SQL queries.
  - `debug_rpc`: Shows details of received RPC requests.
  - `debug_rpc_answer`: Shows details about RPC answers sent back to the client.

## Log Output Destination

- **Default Destination:** Standard output (console screen).
- **Redirecting to a File:** Use `--logfile=<filepath>` to direct logs to a specified file.

## Using the Server Development Options

- **Development Mode Activation:** Enable server-side development mode using `--dev=all`.
- **Features of Development Mode:**
  - **Automatic Python Code Reload:** Changes to Python code are reloaded automatically upon file save, avoiding manual server restarts.
  - **Instant View Definition Changes:** Changes to View definitions are immediately effective, requiring only a browser page reload.
  - **Python Debugger:** The `--dev=all` option activates the pdb Python debugger on exceptions, useful for postmortem analysis. Debugger commands can be found at [Python Debugger Commands](https://docs.python.org/3/library/pdb.html#debugger-commands).
- **Selective Development Options:** The `--dev` option accepts a comma-separated list of options. `all` is usually suitable, but specific debuggers like `ipdb`, `pudb`, and `wpdb` can be used.
- **Automatic Code Reload Requirement:** To use the auto-reload feature, the `watchdog` Python package must be installed. Example: `(env15) $ pip install watchdog`.

## Odoo Commands Quick Reference

- **Configuration File:** `-c, --conf=my.conf` sets the configuration file.
- **Save Config:** `--save` saves the current config file.
- **Stop Server:** `--stop, --stop-after-init` stops the server after module loading.
- **Database Selection:** `-d, --database=mydb` uses a specified database.
- **Database Filter:** `--db-filter=^mydb$` filters databases using a regex.
- **HTTP Port:** `-p, --http-port=8069` sets the HTTP port.
- **Install Modules:** `-i, --init=MODULES` installs a list of modules.
- **Update Modules:** `-u, --update=MODULES` updates a list of modules.
- **Log Level:** `--log-level=debug` sets the log verbosity (e.g., debug, warn).
- **Debugging Options:**
  - `--log-sql`: Debugs SQL calls.
  - `--log-request`: Debugs HTTP request calls.
  - `--log-response`: Debugs responses to HTTP calls.
  - `--log-web`: Debugs HTTP request responses.
  - `--log-handler=MODULE:LEVEL`: Sets log level for a specific module (e.g., `--log-handler=werkzeug:WARN`).
- **Log File:** `--logfile=<filepath>` directs the log to a file.
- **Development Options:** `--dev=OPTIONS` includes options like `all`, debuggers (`pudb`, `wdb`, `ipdb`, `pdb`), `reload`, `qweb`, `werkzeug`, and `xml`.
