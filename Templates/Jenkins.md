## Create text files via command line

```
# 1. Drop old update table
echo 'DROP TABLE SCHEMA.DATABASE.TABLE_UPDATE IF EXISTS'
> 01_MKTG_MA_TABLE_DROP.txt

# 2.Create update table (not listed here)

# 3.Drop old table
echo 'DROP TABLE SCHEMA.DATABASE.TABLE_OLD IF EXISTS'
> 03_MKTG_MA_TABLE_DROP_OLD.txt

# 4.Rename current table to _old table
echo 'ALTER TABLE SCHEMA.DATABASE.TABLE RENAME TO SCHEMA.DATABASE.TABLE_OLD'
> 04_MKTG_MA_TABLE_RENAME_OLD.txt

# 5.Rename newly written update table to be the current table
echo 'ALTER TABLE SCHEMA.DATABASE.TABLE_UPDATE RENAME TO SCHEMA.DATABASE.TABLE' 
> 05_MKTG_MA_TABLE_RENAME_UPDATE.txt

# 6.Generate statistics
echo 'GENERATE STATISTICS ON SCHEMA.DATABASE.TABLE' 
> 06_MKTG_MA_TABLE_GEN_STATS.txt
```
