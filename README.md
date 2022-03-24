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

## WOL

Güttli working out loud:


* [Güttli working-out-loud](https://github.com/guettli/wol)
* [Güttli's opinionated Python Tips](https://github.com/guettli/python-tips)
* [Güttli's Programming Guidelines](https://github.com/guettli/programming-guidelines)

