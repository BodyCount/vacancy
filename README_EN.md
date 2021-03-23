# PHP-dev test task

## Tech stack
 - PHP 7.0+
 - MySQL
 
## Repository structure
 - `load-files` - sample files used in application
 - `schema` - `.sql` database migration files
 - `sources` - application source code

## Application description

This console application parses CSV files and loads data into the database

Application entry point: `sources/cli.php`.
Usage example:

```bash
$ php sources/cli.php load-files/market.eu.20180227
```

This will load data from `load-files/market.eu.20180227` to the `market_data` table,
in specific order:

DB column | CSV column[index]
------------ | -------------
id_value | 0
price | 1
is_noon | 5
update_data | `current date`


## Task description
Two subtasks are given, **execution order matters**
1. Fix an application
1. Add new functionality

#### Feel free to make any refactoring or improvements to the application along the way


# Task1: Fix an application
> This task is limited by fixing `market.eu.%Ymd%` loader

Something happened to the application and it is now malfunctioning: script works but there is no data being loaded to the database

We made some investigation:
 - No errors are coming up during script execution
 - Database gets no data from parsed file
 - There are no `[WARN]`/`[DEBUG]` level logs
 - Log is getting overwritten instead of being appended with new logs.
 
 
You should fix an application:
 
1. Database should be populated with data from `market.eu.20180227`
1. Errors should be handled correctly
1. Logger should **append** logfile and write information about any issues.

# Task2: Add new functionality
>After code change for this task, application should be able to handle **both** `market.eu.%Ymd%` and `market.us.%Ymd%` files.

>This task should be implemented after Task1

New application requirements:
1. Application should be able to handle new market file  - `market.us.%Ymd%`. For this type of files we should load `id_value` for `market_data` table from the last column instead of the first, as it was for `market.eu.` files

1. Use new `markets` table  To validate incoming data. Parsing process should check if `markets` table has incoming `id_value` and skip all rows that have `id_value` not presented in `markets`.
1. `market_data` table should be populated with `update_date` coming from parsed file name. Example: for `market.eu.20180227`, `update_date` should be `2018-02-27`.


1. Format incoming `price` value. Application should remove leading zeros at PHP level.
1. Log number of rows which were loaded during an execution
 
Updated mapping:

 DB column | CSV column[index]
 ------------ | -------------
 id_value | 0 for `market.eu.`, 6 for  `market.us.`
 price | 1
 is_noon | 5
 update_date | `date` from file name
 market_id | `markets.market_id` for incoming `id_value`
 
