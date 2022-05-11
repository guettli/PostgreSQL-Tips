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





## WOL

Güttli working out loud:


* [Güttli working-out-loud](https://github.com/guettli/wol)
* [Güttli's opinionated Python Tips](https://github.com/guettli/python-tips)
* [Güttli's Programming Guidelines](https://github.com/guettli/programming-guidelines)

