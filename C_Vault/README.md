# C_Vault

Simple C-based password vault API with CRUD operations over HTTP. The goal of this project
was really to get better at C. I generally would not use C if my goal was to write an API quickly, and
I imagine that is how most people feel.

I wrote some bash to help manage this API as a systemd service. Bash is really fun once you move beyond scripting
it and into actually programming with it. There is also a Python helper to generate the requests to the API so 
you don't have to wrestle with curl.

Overall, this was a great project to learn how to work with C in an intermediate way. I started with ideas
of memory management that proved to be overengineering for what this turned out to be.
Along the way I learned a lot, but this project is not really that useful. 

It could be... but I'm more interested in other things.

## What this project does

- Stores vault entries in local text files.
- Supports create, read, update, and delete operations.
- Accepts JSON requests and responds with JSON.
- Includes a helper script to install or uninstall a systemd service.

Main files:

- `cli_vault.c`: API server and vault logic.
- `manage_service.sh`: install/uninstall automation for service mode.
- `helper_functions.sh`: shared shell functions used by service script.
- `vault_helper.py`: helper utility script.

## Requirements

- GCC
- cJSON development library
- Linux systemd (only needed for service install script)

## Build

If you just want to run the API and are not interested in Systemd:

```bash
gcc cli_vault.c -o api -I/usr/local/include -L/usr/local/lib -lcjson
```

## Run

```bash
./api
```

The server listens on port `8080`.

## Request format

Send JSON with `entry_name` and a valid `method` (or `action`).

Supported methods/actions:

- `CREATE` or `POST`
- `READ` or `GET`
- `UPDATE` or `PUT` / `PATCH`
- `DELETE`

### Example payloads

Create:

```json
{
	"entry_name": "email",
	"entry_username": "user@example.com",
	"entry_pw": "supersecret",
	"method": "POST"
}
```

Read:

```json
{
	"entry_name": "email",
	"method": "GET"
}
```

Update:

```json
{
	"entry_name": "email",
	"entry_username": "new_user@example.com",
	"entry_pw": "newsecret",
	"method": "PUT"
}
```

Delete:

```json
{
	"entry_name": "email",
	"method": "DELETE"
}
```

## Service script

To setup the API as a complete service,
use the service manager script:

```bash
bash manage_service.sh --install
bash manage_service.sh --uninstall
```

Help:

```bash
bash manage_service.sh --help
```

## Notes

- Vault entries are written as local files in the working directory.
- Do not store production secrets in plain text.
