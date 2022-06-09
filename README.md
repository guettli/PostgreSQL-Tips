# PostgreSQL Tips


## SELECT

### Select rows where a term matches in any column

Like an ad-hoc fulltext index for all columns.

```
select * from mytable where mytable::text ilike '%foo%';
```

Don't do this via code. This is just for manually debugging/introspection.

### Which name is not unique?

You have a table with a primary key "ID" and a column "name".

Which rows have a name which is not unique? Which IDs do these rows have?

```
select name, array_agg(id), count(*) from id_name_table group by name having count(*)>1;
```
Result:
```
 foo    | {100171,100170}        |     2
 bar    | {100384,100654,100670} |     3
```
## Tuning PostgreSQL according to underlaying hardware

https://github.com/le0pard/pgtune

### Compare data two databases

Imagine you have an data-import script, which is written by a former employee who is not reachable any more.

Now you want to check what this script is doing (in your development environment).

The tool [pg_dump_splitsort](https://github.com/akaihola/pgtricks#pg_dump_splitsort) can help.

It takes a dump created by `pg_dump` and splits it into several files. One file per table, each file is sorted, so that
you can use a diff tool like `meld` to compare two directires: 

```
meld old-data/ new-data
```

# psql \x --> expanded table formatting mode

If you want to inspect one row, it is easier for the human eyes to see the columns below each other.

```
psql
\x
select my_table where id = 1234;

-[ RECORD 1 ]---
id       | 1234
foo      | bar
text     | ...
```

# psql \h update

You want to have a short introduction to the syntax of an SQL command?

Example:

```
\h update

Command:     UPDATE
Description: update rows of a table
Syntax:
[ WITH [ RECURSIVE ] with_query [, ...] ]
UPDATE [ ONLY ] table_name [ * ] [ [ AS ] alias ]
    SET { column_name = { expression | DEFAULT } |
          ( column_name [, ...] ) = [ ROW ] ( { expression | DEFAULT } [, ...] ) |
          ( column_name [, ...] ) = ( sub-SELECT )
        } [, ...]
    [ FROM from_item [, ...] ]
    [ WHERE condition | WHERE CURRENT OF cursor_name ]
    [ RETURNING * | output_expression [ [ AS ] output_name ] [, ...] ]

URL: https://www.postgresql.org/docs/13/sql-update.html

```

# Aggregate values in group-by: string_agg()

The table `ticket_labels` is a N:M table: One ticket can have
several labels, and labels can get attached to many tickets.

Task: Show me all IDs of label for each ticket. Skip tickets which only have one label.

```
drop table if exists ticket_labels;

create table ticket_labels (
        ticket_id integer,
        label_id integer
);

insert into ticket_labels 
    values
 (1, 2),
 (1, 3),
 (2, 2),
 (3, 1);

select ticket_id, string_agg(label_id::text, ' ') from ticket_labels group by ticket_id having count(label_id)>1;
```

Result:
```
 ticket_id | string_agg 
-----------+------------
         1 | 2 3
```


# Raise Exception on INSERT/UPDATE

Imagine you work on a large code base which is new to you.

The code somewhere updates a table, but you don't know which particular
code lines execute the update.

You can create a trigger which raises an exception. This way you can find
the corresponding code easily:

```
CREATE FUNCTION yell() RETURNS trigger
   LANGUAGE plpgsql AS
$$BEGIN
   RAISE EXCEPTION 'yell';
END;$$;

CREATE TRIGGER yell_trigger
   BEFORE UPDATE OR INSERT ON your_table FOR EACH ROW
   EXECUTE PROCEDURE yell();
```

When you execute the code, you will get an exception and a stracktrace which shows which line of code modifies `your_table`.

To remove the trigger:

```
DROP TRIGGER yell_trigger ON your_table;
```




## WOL

Güttli working out loud:


* [Güttli working-out-loud](https://github.com/guettli/wol)
* [Güttli's opinionated Python Tips](https://github.com/guettli/python-tips)
* [Güttli's Programming Guidelines](https://github.com/guettli/programming-guidelines)

