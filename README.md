# Auidted Lua Triggers

An audited lua trigger is designed to automate the most common use of lua triggers: writing the event information of each insert, update and/or delete on a given table to another table. Using audited lua triggers, you can perform this task with one line, simply specifying the name of the trigger, the table you are auditng, and the types of updates you want to subscribe to.

```sql
CREATE LUA TRIGGER trigger_name ON (TABLE table_name ON INSERT AND UPDATE AND DELETE)
```
