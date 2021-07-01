# Auidted Lua Triggers

An audited lua trigger is designed to automate the most common use of lua triggers: writing the event information of each insert, update and/or delete on a given table to another table. Using audited lua triggers, you can perform this task with one line, simply specifying the name of the trigger, the table you are auditng, and the types of updates you want to subscribe to. To create an audited trigger, use the following form.

```sql
CREATE LUA TRIGGER *triggername* ON (TABLE *tablename* ON INSERT AND UPDATE AND DELETE) AUDITED
```

When you create an audited lua trigger, the data is stored in a table of the form "$audit_*tablename*".
