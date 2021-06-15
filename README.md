# PostgreSQL Tips


## SELECT

### Select rows where a term matches in any column

Like an ad-hoc fulltext index for all columns.

```
select * from mytable where mytable::text ilike '%foo%';
```

Don't do this via code. This is just for manually debugging/introspection.


## WOL

Güttli working out loud:


* [Güttli working-out-loud](https://github.com/guettli/wol)
* [Güttli's opinionated Python Tips](https://github.com/guettli/python-tips)
* [Güttli's Programming Guidelines](https://github.com/guettli/programming-guidelines)

