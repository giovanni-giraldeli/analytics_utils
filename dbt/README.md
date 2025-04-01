## Table of contents
- **[Basic Commands](#basic-commands)**<br>
  - **[dbt run](#dbt-run)**<br>
  - **[dbt build](#dbt-build)**<br>
  - **[dbt test](#dbt-test)**<br>
  - **[dbt \<cmd> --select <model_name>](#dbt---cmd----select--model-name-)**<br>
- **[System Architecture](#system-architecture)**<br>
  - **[Models](#models)**<br>
  - **[Tests](#tests)**<br>
  - **[Macros](#macros)**<br>
- **[Jinja](#jinja)**
  - **[Basic Syntax](#basic-syntax)**
  - **[Basic Functions](#basic-functions)**
---

# Basic Commands
- ### dbt run
  It will run the jobs in the pipeline
- ### dbt build
  It will run the tests and afterward it will run the jobs
- ### dbt test
  It will run only the tests in the pipeline
- ### dbt \<cmd> --select <model_name>
  It will run only the selected model

# System Architecture
- ### Models
  These are the scripts that will generate your tables at the end. Each model should generate a single output and that should be reused in the project.<br>
  In other words, these objects leverage modularity and DRY across the project.
  
  - **Scripts**<br>
    The ```.sql``` scripts to create your objects lie here.<br>
    The name of your script will determine the name of your object.<br>
    You should save your SQL scripts here as ```SELECT``` statements and dbt will generate a object afterwards based on how you configured the environment, possibly being a table or a view (defaulted to view).<br>
    > _**Pro tip:** make sure that your sources live here as well! This will make sure they are considered in the lineage and will enforce the modularity and DRY concepts!_

  - **Configuration files**  
    You can create ```.yml``` files to set specific configurations to your models, such as data tests, freshness tests, and metadata (table descriptions, column descriptions).<br>
    Here's the place to configure where the source data lives, specifying the dabatase, schema and table names in order to enable it to be referenced in the models.
    - Documentation file<br>
      In order to keep the configuration files more organized and with shorter content,
      you can create more detailed documentation creating a ```.md``` with the content and wrapping it into a ```docs``` Jinja function.<br>
      This will enable you to reference the documentation in the configuration file calling it with Jinja.

- ### Tests
  dbt already provides some pre-built tests (not null, unique, accepted values and relationships), but if more specific data tests, you can create a script to execute these tests.<br>
  This script should be saved as a ```.sql``` file and saved in the tests folder in the dbt root folder.

- ### Macros
  This is another feature that enhance DRY in your project.<br>
  Here you can create functions based on Jinja to use in the models, in order to standardize the outputs and reduce maintenance effort.

# Jinja
This is the scripting language used in dbt. It's a pythonic language, so the syntax is similar to it.<br>
It's used mostly to call functions and deal with references, such as loops, ifs, lists, arrays, set variables and even call external functions.<br>
There's a dbt shared repo of Jinja functions and we can reuse them by creating a ```packages.yml``` in dbt root folder and referencing there the desired project from the shared repo.
- ### Basic syntax
  - ```{% ... %}```: indicate the start and the end of the Jinja statement.
    - ```{%- ... -%}```: adding the ```-``` sign trims the whitespace.
  - ```{{ <var> }}```: calls the Jinja value set previously.
  - ```{# ... #}```: indicate the start and the end of a multi line comment block. The content will not appear in the compiler.
- ### Basic functions
  - ```{% ref('<model>') %}```: references an existing model in the project. This is how the DAG and the lineage are created.
  - ```{% source('<source_name>', '<model_name>') %}```: references an existing source in the project and the source model, which will reference to the database/schema and table, respectively.
  - ```{% if <logical_conditions> %}```: starts an ```if``` statement. Needs to end with a ```{% endif %}``` command.
  - ```{% for <for_loop_condition> %}```: starts a ```for``` loop. Needs to end with a ```{% endfor %}``` command.
  - ```{% set <variable_name> %}```: set the value to the variable. Needs to end with a ```{% endset %}``` command.
  - ```{% macro <macro_name>(<macro_paramaters>)%}```: creates a macro. Needs to end with a ```{% endmacro %}``` command.
  - ```{% docs <doc_name> %}```: starts creating a documentation that be referenced in a ```.yml``` file. Needs to end with a ```{% enddoc %}``` command.
  - ```{% log(<message>, info=True) %}```: log a message while executing the script. You can use it in inside the macro to log specific messages depending on the choosen path.
