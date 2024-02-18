# Отслеживание выполнения
https://postgrespro.ru/docs/postgresql/15/progress-reporting

## Отслеживание выполнения CREATE INDEX
```sql
SELECT
    a.query,
    p.phase,
    round(p.blocks_done / p.blocks_total::numeric * 100, 2) AS "% done",
    p.blocks_total,
    p.blocks_done,
    p.tuples_total,
    p.tuples_done,
    ai.schemaname,
    ai.relname,
    ai.indexrelname
FROM
    pg_stat_progress_create_index p
    inner join pg_stat_activity a ON p.pid = a.pid
    left join pg_stat_all_indexes ai on ai.relid = p.relid AND ai.indexrelid = p.index_relid;
```

## Отслеживание выполнения VACUUM
```sql
select
    a.query,
    round(p.heap_blks_scanned / p.heap_blks_total::numeric * 100, 2) AS "% done",
    p.*
from
    pg_stat_progress_vacuum p
    inner join pg_stat_activity a ON p.pid = a.pid;
```
