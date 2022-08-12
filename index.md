# The most useful command of all
**Help > Find Action**

# Aim of this document
This is an opinionated list of Pycharm configurations and tools that are useful on an everyday basis whn working on paylead_flask project.
It is far from being an exhaustive list of pycharm functionalities
There is a keyboard shortcut for mostly everything in pycharm.
Shortcut are purposely not indicated because default values change between environments and shortcuts are configurable.

# Python environment
settings > Project:... > PythonInterpreter
: To create a new virtual env or add an existing one to pycharm project

If the virtual env already exists: select the python executable of the virtual env, usually in `nameofvenv/bin/python3.x`
If you want to create a new virtual env, select the python executable of the base environment

The virtual env associated to the project will be used for:
- check if dependencies are installed
- execute python code through pycharm
- activate the correct python env when opening a new terminal in pycharm

# External tools
settings > Tools > External tools
: To set up black / flake8 / isort you can configure 'external tools'

External tool are commands that pycharm can execute

Once an external tool is configured, you can associate it to a keyboard shortcut or a file watcher to execute it automatically on file change or save

## Configuration examples:

### blake
*requires to have blake installed in your project virtual env*

name
: black

description
: black

Program
: $PyInterpreterDirectory$/black

Arguments
: --config pyproject.toml $FilePath$

Working directory $ProjectFileDir$

Advanced Options:

[x] Synchronize files after execution

[ ] Open console for tool output

### isort
*requires to have isort installed in your project virtual env*

name
: isort

description
: isort

Program
: $PyInterpreterDirectory$/isort

Arguments
: $FilePath$

Working directory
: $ProjectFileDir$

Advanced Options:

- [x] Synchronize files after execution

- [] Open console for tool output

### flake8
name
: flake8

description
: flake8

Program
: $PyInterpreterDirectory$/python

Arguments
: -m flake8 --config $ProjectFileDir$/.flake8 $FilePath$

Working directory
: $ProjectFileDir$

Advanced Options:

- [x] Synchronize files after execution

- [] Open console for tool output

Output filters:
```
$FILE_PATH$\:$LINE$\:$COLUMN$\:.*
$FILE_PATH$\:$LINE$\:.*
```

## Using external tools
Tools > External Tools
: run an external tool


settings > Tools > Actions on Save
: configure running an external tool on file save (you may want this for black and isort)


settings > Keymap > external tools
: configure a shortcut for an external tool


settings > Tools > File Watchers
: configure running an external tool on file change (useful mostly if the file is changed externally)


To define on which files an external tool will run, use 'scopes'

# Scopes
settings > appearance & behavior > scopes
: Configure scopes

Scope are simply user-defined file filters
Scopes are useful for:
- filter files in the left panel "project files"
- quickly filter search results
- restrict the files on which an external tool will run

### Example of scopes
file\[paylead_flask\]:core/apps/api//*
: all files in 'api' folder


file\[paylead_flask\]:core//*.py
: all python files in 'core'

# Configurations
Configurations are predefined commands that the user can choose to execute (comparable to what the make file does)
configurations are useful for:
- save complex command lines
- define environment variables before that have to be set before executing the command
- run and debug tests
- run and debug API and change the database used by the API

## Configuration to debug flask API with local DB
Add a 'flask server' configuration and use:

- [x] script path

Target
: path_to_paylead_flask/core/apps/api/main.py


Application
: app


Additional options
: --host=127.0.0.1 --port=5000


FLASK_ENV
: development

- [x] FLASK_DEBUG

Environment variables: 
```
CELERY_BROKER_URL=pyamqp://localhost;
FLASK_APP=core.apps.api.main;
SECRET_KEY=none;
SERVER_NAME=localhost:5000;
SQLALCHEMY_DATABASE_URI=postgresql://postgres@/paylead
```

run the configuration with the debugger. During execution the code will stop at breakpoints

# Database
Database panel is on the right-hand side

settings > language & framework > SQL dialect
: set how the IDE will handle SQL


Global SQL Dialect
: PostgreSQL. If you use another database in the code (e.g. clickhouse) you can set specific path to use another dialect

## Add a connection to a database
'+' > Datasource > postgresSQL
: Configure database connection parameters

Host:
:  localhost # if you connect to integration / acceptance / sandbox / db-off through ssh, Host is still localhost


Authentication
: "User and password"

## List tables and views
You can navigate the database structure in the database panel.

You can display the content of a table by double-clicking it

Right-click on a table to see options to inspect and edit the definition of the table

*right-click on a foreign key cell > go to*
: enables to navigate to referenced / referencing rows


If a schema is not showing: 

*right-click on the datasource* > Data Source Properties > Schemas 
: check schemas you want to be shown




## Execute SQL
*click on a database* > jump to query console > new query console
: opens a new SQL console for the selected database

Type your query. Pycharm can autocomplete table names and join conditions.

If there are no `join` or `group by` in the request, the result table **is editable** and support copy-pasting

You can also execute SQL directly from a project file (*right click on sql text* > execute)

> :warning: **SQL console are not saved like files**: If you close the console or the connection to the database, **you will lose your work**. Use them as disposable scrap book.

# Environment variables
There are two main ways to define environment variable in Pycharm for code execution:

1- through configuration, in field `environment variables`

2- through the integrated terminal

## environment variables in configurations
It is useful to set env vars in configuration so different configurations can have different environments.

You can also define env vars in the configuration template so all newly created configurations will share the same env vars

## environment variables in terminal
settings > Tools > Terminal > Environment variables
: Define environment variables for each new terminal.

It is specially useful to set:

```
CELERY_BROKER_URL=pyamqp://localhost;
FLASK_APP=core.apps.api.main;
SECRET_KEY=none;SERVER_NAME=localhost:5000;
SQLALCHEMY_DATABASE_URI=postgresql://postgres@/paylead
```

so you can run `flask shell` directly from a newly created terminal

Do not change the `PYTHON_PATH` value, or you will break your virtual env configuration

# Plugins
GitToolBox
: add inline git blames
