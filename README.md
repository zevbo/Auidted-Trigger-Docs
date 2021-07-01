# Auidted Triggers

An audited lua trigger is designed to automate the most common use of lua triggers: writing the event information of each insert, update and/or delete on a given table to another table. Using audited lua triggers, you can perform this task with one line, simply specifying the name of the trigger, the table you are auditng, and the types of updates you want to subscribe to. To create an audited trigger, you simply use a normal ```CREATE LUA TRIGGER``` statement except add the word ```AUDITED``` to the end. For instance, if you wanted an audited trigger of name "example_triggernmane" to fire on inserts and deletes of table "example_tablename," you simply need the following line:

```sql
CREATE LUA TRIGGER example_triggername ON (TABLE example_tablename ON INSERT AND UPDATE) AUDITED
```

When you create an audited lua trigger, the data is stored in by default stored in a table with name "$audit_*tablename*". If such a table already exists, it will create a table with the same name except with the smallest possible integer greater than or 2 appended to the end of it that makes its name unique. 

All audit tables have the following three non-nullable fields:
```sql
type cstring(4), tbl cstring(64), logtime datetime
```

For a given table with columns col1, ..., coln, of types type1, ..., typen, it's audit table will have the following nullable fields in addition to the three standard ones:
```sql
new_col1 type1, ..., new_coln typen, old_col1 type1, ..., old_coln typen
```

This table is by default read only. If you would like the ability to write to this table, use the type ```WRITEABLE_AUDITED``` instead of ```AUDITED```.

On schema changes to the subscribed table, the audit_table will automatically undergo a re-adjustment as well. For type changes, the types of the corresponding old_ and new_ fields will have the same type change. For field name changes, the corresponding text after the old_ and new_ of the corresponding audit table fields will change in the same way. Field deletions are slightly more complicated, as we do not want to get rid of any old data. For most field deletions, we simply rename the corresponding old_ and new_ fields to also have a prefix of \_archived. So, if you deleted a table called nums, the audit tables fields will go from being new_nums and old_nums, to new_nums_archived and old_nums_archived. This is unless you archive a field with the same name more than once, meaning an old_fieldname_archived and new_fieldname_archived already exists. If this is the case, it will simply append the smallest possible integer not taken to the end of it, starting with 2.

There is also the question of how dropping works for the subcrscribed table, the audit_table, the procedure and trigger.
- subscribed table : here, the audited trigger acts exactly the same as a normal trigger
- audit_table : if the audit_table gets deleted, so does the procedure and the trigger
- procedure : if the procedure gets deleted, so does the trigger but the table remains
- trigger: if the trigger gets deleted, nothing else is effected [note: might want to delete the procedure]

