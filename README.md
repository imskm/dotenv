# dotenv
A simple `.env` file parser and environment varialble loader for [kore][https://kore.io] application.

## Integration
* Download this repository, extract it, and put the `dotenv` folder inside your `kore` application's `src` directory.
* Then call `dotenv_init()` from inside `kore_worker_configure()` and `dotenv_teardown()` from inside `kore_worker_teardown()`.
  For example, write the below code inside any of your application's `.c` file. It might be cleaner if you write your all initialisation code in
  seperate `.c` file like `init.c` and put all your init code like `pgsql` db registeration call, etc. inside this file.

```c
/* This include is required to use kore hooks API in kore-4.1.0 */
#include <kore/hooks.h>

#include "dotenv/dotenv.h"

...

void
kore_worker_configure(void)
{
	dotenv_init();
}

void
kore_worker_teardown(void)
{
	dotenv_teardown();
}
```

* Finally create `.env` file in your `kore` project's root directory and write each environment variable in seperate line, for example:
```
DB_HOST=127.0.0.1
DB_USERNAME=my_pgsql_db_username
DB_PASSWORD=my_pgsql_db_password
DB_NAME=my_db_name

MAIL_USERNAME=my_mail_username
...
```
That's it, now run your kore app.

## Accessing environment variable
Call standard `getenv()` function to access your environment variables that you stored in `.env` file;

## Content format of `.env` file
* Each line should have `<key>=<value>` format
* `"` (double quote) or `'` (single quote) arround `<value>` is considered part of value. So if you have a value with space then just right it as it is.
* You can add comment line in `.env` file. Comment line must start with `#` character. `#` in the middle is not considered as comment line.
* If any of the line has error, invalid format or failed to set variable in process's environment then you can find the error on console.

