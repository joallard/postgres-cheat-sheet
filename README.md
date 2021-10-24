# Postgres Stuff Cheat Sheet

## Regex

### Match

Use the `~` operator.

> ## 9.7.3. POSIX Regular Expressions
>
> **Table 9-12. Regular Expression Match Operators**
>
> | Operator | Description                                                  | Example                               |
> | -------- | ------------------------------------------------------------ | ------------------------------------- |
> | `~`      | Matches regular expression, case sensitive                   | `'thomas' ~            '.*thomas.*'`  |
> | `~*`     | Matches regular expression, case insensitive                 | `'thomas' ~*            '.*Thomas.*'` |
> | `!~`     | Does not match regular expression, case            sensitive | `'thomas' !~            '.*Thomas.*'` |
> | `!~*`    | Does not match regular expression, case            insensitive | `'thomas' !~*            '.*vadim.*'` |



## String

### Presence

For when your stringish column might contain the empty string as well as nulls.

| `string` | `string = ''` | `string = '' is false` | `string = '' is not false` |
|---|---|---|---|
| *Original string* | *String is empty* | *We are sure the string is not empty (empty is false)* | *'String is empty' is something other than 'surely false'* |
| `'a'` | `false` | `true` | `false` |
| `''` | `true` | `false` | `true` |
| `null` | `null` | `false` | `true` |

String is present: `string = '' is false`

String is blank: `string = '' is not false`

### Split

`string_to_array(anyarray, delimiter [, nullstring])`

### Trim

`trim(string)`

```sql
=# select 'a' || trim(' u v ') || 'z';
 ?column?
----------
 au vz
(1 row)
```





## Array

### Compact

Ruby `array.compact!`

PG `array_remove(array, null)`

### Length

`array_length(array, 1)`

### Join

Ruby `array.join(delimiter)`

PgSQL `array_to_string(array, delimiter)`



## JSON

### Fetch

`json->'key'` (returns JSON)

`json->>'key'` (returns text)



> The `->` operator returns a `json` result. Casting it to `text` leaves it in a json reprsentation.
>
> The `->>` operator returns a `text` result. Use that instead.
>
> ```sql
> test=> SELECT '{"car": "going"}'::jsonb -> 'car';
>  ?column? 
> ----------
>  "going"
> (1 row)
>
> test=> SELECT '{"car": "going"}'::jsonb ->> 'car';
>  ?column? 
> ----------
>  going
> (1 row)
> ```
>
> *[Answer](https://stackoverflow.com/a/40928412/720164) from Craig Ringer on Stackoverflow: "Remove double quotes from the return of a function in PostgreSQL"*

## Schema Editing

```sql
ALTER TABLE distributors ADD PRIMARY KEY (dist_id);

ALTER TABLE distributors RENAME TO suppliers;



ALTER TABLE foo
    ALTER COLUMN foo_timestamp SET DATA TYPE timestamp with time zone
    USING timestamp with time zone 'epoch' + foo_timestamp * interval '1 second' -- if needed
;

-- short: ALTER column TYPE newtype
```

### Create Index
```sql
CREATE INDEX tbl_col_idx ON table_name (column_name);
 -- [USING] method
```


