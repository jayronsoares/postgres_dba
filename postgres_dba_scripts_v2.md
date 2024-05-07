## These scripts provide valuable insights into PostgreSQL database health, performance, and replication status.

1. **Database Health Check Script:**
   - **Purpose:** Checks the overall health of PostgreSQL databases, including disk usage, table bloat, and replication status.
   - **Script:**
     ```bash
     #!/bin/bash
     
     # Check disk usage
     df -h

     # Check table bloat
     psql -c "SELECT schemaname, relname, pg_size_pretty(pg_total_relation_size(oid)) AS total_size FROM pg_catalog.pg_stat_user_tables ORDER BY pg_total_relation_size(oid) DESC;"

     # Check replication status
     psql -c "SELECT application_name, state, sync_state FROM pg_stat_replication;"
     ```

2. **Query Performance Analyzer:**
   - **Purpose:** Analyzes query performance by identifying slow queries and their execution plans.
   - **Script:**
     ```sql
     SELECT query, total_time, calls, rows, 100.0 * shared_blks_hit / nullif(shared_blks_hit + shared_blks_read, 0) AS hit_percent
     FROM pg_stat_statements
     ORDER BY total_time DESC
     LIMIT 10;
     ```

3. **Lock Monitoring Script:**
   - **Purpose:** Monitors locks held by transactions to identify lock contention issues.
   - **Script:**
     ```sql
     SELECT pid, usename, datname, relation::regclass, mode, granted
     FROM pg_locks
     ORDER BY relation;
     ```

4. **WAL Archiving Status Script:**
   - **Purpose:** Checks the status of WAL archiving to ensure continuous replication.
   - **Script:**
     ```sql
     SELECT archived_count, last_archived_wal, last_archived_time
     FROM pg_stat_archiver;
     ```

5. **Database Replication Lag Script:**
   - **Purpose:** Monitors replication lag between primary and standby servers.
   - **Script:**
     ```sql
     SELECT pg_current_xlog_location() - pg_last_xlog_replay_location() AS replication_delay;
     ```

6. **Database Backup Verification Script:**
   - **Purpose:** Verifies the integrity of database backups to ensure recoverability.
   - **Script:**
     ```bash
     pg_restore --list backup_file.backup
     ```

7. **Disk Space Usage Script:**
   - **Purpose:** Monitors disk space usage of PostgreSQL data directories.
   - **Script:**
     ```bash
     du -h /path/to/postgresql/data
     ```

8. **Checkpoint Activity Script:**
   - **Purpose:** Monitors checkpoint activity to optimize performance and recovery times.
   - **Script:**
     ```sql
     SELECT checkpoint_id, checkpoint_time, redo_size
     FROM pg_stat_bgwriter
     ORDER BY checkpoint_time DESC
     LIMIT 10;
     ```

9. **Vacuum Analysis Script:**
   - **Purpose:** Analyzes vacuum activity to identify tables requiring maintenance.
   - **Script:**
     ```sql
     SELECT relname, last_vacuum, last_autovacuum, vacuum_count, autovacuum_count
     FROM pg_stat_user_tables
     ORDER BY last_vacuum DESC
     LIMIT 10;
     ```

10. **Long Running Transactions Script:**
    - **Purpose:** Identifies transactions holding locks for an extended period, potentially causing deadlock situations.
    - **Script:**
      ```sql
      SELECT pid, age(clock_timestamp(), query_start) AS duration, query
      FROM pg_stat_activity
      WHERE state = 'active'
      AND query NOT ILIKE '%pg_stat_activity%'
      AND query_start < clock_timestamp() - interval '5 minutes';
      ```
