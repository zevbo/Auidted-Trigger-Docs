# Auidted Triggers

An audited lua trigger is designed to automate the most common use of lua triggers: writing the event information of each insert, update and/or delete on a given table to another table. Using audited lua triggers, you can perform this task with one line, simply specifying the name of the trigger, the table you are auditng, and the types of updates you want to subscribe to. To create an audited trigger, you simply use a normal ```CREATE LUA TRIGGER``` statement except add the word ```AUDITED``` to the end. For instance, if you wanted an audited trigger of name "example_triggernmane" to fire on inserts and deletes of table "example_tablename," you simply need the following line:

```sql
CREATE LUA TRIGGER example_triggername ON (TABLE example_tablename ON INSERT AND UPDATE) AUDITED
```

When you create an audited lua trigger, the data is stored in a table of the form "$audit_*tablename*". All the tables have the following three non-nullable fields:
```sql
type cstring(4), tbl cstring(64), logtime datetime
```

For a given table with columns col1, ..., coln, of types type1, ..., typen, it's audit table will have the following nullable fields in addition to the three standard ones:
```sql
new_col1 type1, ..., new_coln typen, old_col1 type1, ..., old_coln typen
```

This table is by default read only. If you would like the ability to write to this table, use the keyword ```UNSAFE_AUDITED``` instead.
