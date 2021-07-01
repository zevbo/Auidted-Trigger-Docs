# Auidted Triggers

An audited lua trigger is designed to automate the most common use of lua triggers: writing the event information of each insert, update and/or delete on a given table to another table. Using audited lua triggers, you can perform this task with one line, simply specifying the name of the trigger, the table you are auditng, and the types of updates you want to subscribe to. To create an audited trigger, use the following form.

```sql
CREATE LUA TRIGGER example_triggername ON (TABLE example_tablename ON INSERT AND UPDATE AND DELETE) AUDITED
```

When you create an audited lua trigger, the data is stored in a table of the form "$audit_*tablename*". All the tables have the following three non-nullable fields:
```sql
type cstring(4), tbl cstring(64), logtime datetime
```

For a given table with columns col1, ..., coln, of types type1, ..., typen, it's audit table will have the following nullable fields in addition to the three standard ones:
```sql
new_col1 type1, ..., new_coln typen, old_col1 type1, ..., old_coln typen
```
