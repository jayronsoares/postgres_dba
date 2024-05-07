## These scripts: cover critical aspects such as index usage, transaction management, configuration review, 
## temporary file usage, and active queries, providing comprehensive visibility into PostgreSQL database

11. **Index Usage Analysis Script:**
    - **Purpose:** Analyzes index usage to identify unused or underutilized indexes.
    - **Script:**
      ```sql
      SELECT schemaname, relname, indexrelname, idx_scan, idx_tup_read, idx_tup_fetch
      FROM pg_stat_user_indexes
      ORDER BY idx_scan ASC;
      ```

12. **Transaction ID Wraparound Prevention Script:**
    - **Purpose:** Monitors transaction ID wraparound risk to prevent database downtime.
    - **Script:**
      ```sql
      SELECT datname, age(datfrozenxid) AS xid_age
      FROM pg_database
      WHERE NOT datallowconn;
      ```

13. **Database Configuration Review Script:**
    - **Purpose:** Reviews PostgreSQL configuration parameters for potential optimizations and compliance with best practices.
    - **Script:**
      ```sql
      SHOW ALL;
      ```

14. **Temporary Files Usage Script:**
    - **Purpose:** Monitors temporary file usage to prevent disk space exhaustion.
    - **Script:**
      ```sql
      SELECT temp_files, temp_bytes
      FROM pg_stat_database
      WHERE datname = 'your_database';
      ```

15. **Active Queries Script:**
    - **Purpose:** Lists active queries along with their execution time to identify long-running or resource-intensive queries.
    - **Script:**
      ```sql
      SELECT pid, usename, query_start, now() - query_start AS duration, query
      FROM pg_stat_activity
      WHERE state = 'active'
      ORDER BY duration DESC;
      ```

16. **Index Usage Analysis Script:**
    - **Purpose:** Analyzes index usage to identify unused or underutilized indexes.
    - **Script:**
      ```sql
      SELECT schemaname, relname, indexrelname, idx_scan, idx_tup_read, idx_tup_fetch
      FROM pg_stat_user_indexes
      ORDER BY idx_scan ASC;
      ```

17. **Long Running Queries Script:**
    - **Purpose:** Identifies queries that have been running for an extended period, potentially indicating performance issues.
    - **Script:**
      ```sql
      SELECT pid, now() - pg_stat_activity.query_start AS duration, query
      FROM pg_stat_activity
      WHERE state = 'active'
      AND now() - pg_stat_activity.query_start > interval '5 minutes'
      ORDER BY duration DESC;
      ```

18. **Streaming Replication Lag Script:**
    - **Purpose:** Monitors replication lag specifically for streaming replication setups.
    - **Script:**
      ```sql
      SELECT pg_current_xlog_location() - pg_last_xlog_receive_location() AS replication_lag_bytes;
      ```

19. **Database Table Fragmentation Script:**
    - **Purpose:** Identifies fragmented tables that may require vacuuming or reindexing.
    - **Script:**
      ```sql
      SELECT schemaname, relname, pg_size_pretty(pg_total_relation_size(relid)) AS total_size
      FROM pg_catalog.pg_statio_user_tables
      ORDER BY pg_total_relation_size(relid) DESC;
      ```

20. **PostgreSQL Configuration Check Script:**
    - **Purpose:** Checks PostgreSQL configuration settings against best practices and potential performance bottlenecks.
    - **Script:**
      ```bash
      pg_settings;
      ```
