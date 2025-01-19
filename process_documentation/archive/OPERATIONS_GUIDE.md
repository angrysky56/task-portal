# Task Portal System Operations Guide

## Daily Operations

### System Startup Sequence

1. **Pre-startup Checks**

   ```bash
   # Verify database availability
   uvx verify-db-connection --path "F:/ai_workspace/Nexus-Prime/NEXUS_PRIME.db"
   
   # Check Neo4j status
   neo4j status
   
   # Verify Prover9 availability
   test -f "F:/Prover9-Mace4/bin-win32/prover9"
   ```

2. **Server Initialization**

   ```bash
   # Start MCP servers in sequence
   
   # 1. Start SQLite server
   uv run mcp-server-sqlite --db-path "F:/ai_workspace/Nexus-Prime/NEXUS_PRIME.db"
   
   # 2. Start Neo4j server
   uv run mcp-neo4j-cypher --db-url "bolt://localhost:7687" --username neo4j --password ********
   
   # 3. Start Sequential Thinking server
   npx @modelcontextprotocol/server-sequential-thinking
   
   # 4. Start Task Manager
   npx @kazuph/mcp-taskmanager
   
   # 5. Start MCP Logic server
   uv run mcp_logic --prover-path "F:/Prover9-Mace4/bin-win32"
   ```

3. **System Health Verification**

   ```sql
   -- Check sequential process bridge
   SELECT process_id, processing_status 
   FROM sequential_process_bridge 
   WHERE processing_status != 'COMPLETED'
   ORDER BY created_at DESC LIMIT 5;
   
   -- Verify logical process states
   SELECT bridge_id, verification_level 
   FROM logical_process_bridge 
   WHERE coherence_metric < 0.8;
   
   -- Check pending tasks
   SELECT integration_id, verification_status 
   FROM task_portal_integration 
   WHERE verification_status = 'PENDING';
   ```

## Operational Workflows

### 1. Task Processing Pipeline

```python
async def monitor_task_pipeline():
    # Check task queue health
    queue_status = await read_query("""
        SELECT COUNT(*) as pending_tasks,
               AVG(coherence_level) as avg_coherence
        FROM sequential_process_bridge
        WHERE processing_status = 'PENDING'
    """)
    
    # Monitor verification status
    verification_status = await read_query("""
        SELECT verification_status, COUNT(*) as status_count
        FROM task_portal_integration
        GROUP BY verification_status
    """)
    
    return {
        "queue_status": queue_status,
        "verification_metrics": verification_status
    }
```

### 2. Sequential Thinking Management

```python
async def manage_sequential_processes():
    # Monitor active thought sequences
    active_sequences = await read_query("""
        SELECT process_id, current_thought_number, total_thoughts
        FROM sequential_process_bridge
        WHERE processing_status = 'IN_PROGRESS'
    """)
    
    # Check coherence metrics
    coherence_metrics = await read_query("""
        SELECT process_id, coherence_level
        FROM sequential_process_bridge
        WHERE coherence_level < 0.9
    """)
    
    return {
        "active_processes": active_sequences,
        "low_coherence_processes": coherence_metrics
    }
```

### 3. Logical Verification Management

```python
async def monitor_logical_verification():
    # Check verification pipeline
    verification_status = await read_query("""
        SELECT logical_state_id, verification_level
        FROM logical_process_bridge
        WHERE verification_level < 4
    """)
    
    # Monitor axiom validation
    axiom_status = await prove({
        "premises": [
            "all x (ProcessValid(x) -> VerificationComplete(x))"
        ],
        "conclusion": "all x (SystemHealthy(x))"
    })
    
    return {
        "verification_status": verification_status,
        "axiom_validation": axiom_status
    }
```

## System Monitoring

### 1. Health Checks

```python
async def system_health_check():
    # Database connection check
    db_health = await verify_connections()
    
    # Process coherence check
    coherence_status = await read_query("""
        SELECT AVG(coherence_level) as avg_coherence,
               MIN(coherence_level) as min_coherence,
               MAX(coherence_level) as max_coherence
        FROM sequential_process_bridge
    """)
    
    # Verification pipeline check
    verification_health = await read_query("""
        SELECT COUNT(*) as total_verifications,
               SUM(CASE WHEN verification_status = 'FAILED' THEN 1 ELSE 0 END) as failed_verifications
        FROM task_portal_integration
    """)
    
    return {
        "database_health": db_health,
        "coherence_metrics": coherence_status,
        "verification_metrics": verification_health
    }
```

### 2. Performance Monitoring

```python
async def monitor_performance():
    # Process timing metrics
    timing_metrics = await read_query("""
        SELECT AVG(JULIANDAY(completed_at) - JULIANDAY(created_at)) * 86400 as avg_process_time
        FROM sequential_process_bridge
        WHERE processing_status = 'COMPLETED'
    """)
    
    # System load metrics
    load_metrics = {
        "active_processes": await count_active_processes(),
        "pending_verifications": await count_pending_verifications(),
        "coherence_checks": await count_coherence_checks()
    }
    
    return {
        "timing_metrics": timing_metrics,
        "load_metrics": load_metrics
    }
```

## Error Recovery

### 1. Common Issues and Solutions

#### Database Connection Issues

```python
async def recover_database_connection():
    try:
        # Attempt reconnection
        await establish_sqlite_connection(
            "F:/ai_workspace/Nexus-Prime/NEXUS_PRIME.db"
        )
        
        # Verify connection
        await read_query("SELECT 1")
        
    except Exception as e:
        # Log error and attempt fallback
        await log_error(e)
        await initialize_fallback_database()
```

#### Verification Pipeline Failures

```python
async def recover_verification_pipeline():
    # Identify failed verifications
    failed_verifications = await read_query("""
        SELECT integration_id, process_id
        FROM task_portal_integration
        WHERE verification_status = 'FAILED'
    """)
    
    for verification in failed_verifications:
        # Reset verification state
        await write_query("""
            UPDATE task_portal_integration
            SET verification_status = 'PENDING'
            WHERE integration_id = ?
        """, [verification['integration_id']])
        
        # Reinitialize verification process
        await initialize_verification_pipeline(verification['process_id'])
```

#### Coherence Recovery

```python
async def recover_coherence():
    # Identify low coherence processes
    low_coherence = await read_query("""
        SELECT process_id
        FROM sequential_process_bridge
        WHERE coherence_level < 0.7
    """)
    
    for process in low_coherence:
        # Reinitialize sequential thinking
        await reinitialize_sequential_thinking(process['process_id'])
        
        # Verify new coherence level
        await verify_process_coherence(process['process_id'])
```

## Performance Optimization

### 1. Database Optimization

```sql
-- Optimize indexes
CREATE INDEX IF NOT EXISTS idx_process_status 
ON sequential_process_bridge(processing_status);

CREATE INDEX IF NOT EXISTS idx_verification_status 
ON task_portal_integration(verification_status);

-- Analyze query performance
ANALYZE sequential_process_bridge;
ANALYZE logical_process_bridge;
ANALYZE task_portal_integration;
```

### 2. Process Optimization

```python
async def optimize_process_pipeline():
    # Batch similar processes
    await batch_sequential_processes()
    
    # Optimize verification pipeline
    await optimize_verification_queue()
    
    # Clean up completed processes
    await cleanup_completed_processes()
```

### 3. Memory Management

```python
async def manage_system_memory():
    # Clear process cache
    await clear_process_cache()
    
    # Optimize database memory usage
    await optimize_database_memory()
    
    # Clean up temporary resources
    await cleanup_temporary_resources()
```

## Daily Maintenance Checklist

1. **Morning Checks**
   - [ ] Verify all server connections
   - [ ] Check process queue status
   - [ ] Monitor coherence metrics
   - [ ] Verify verification pipeline health

2. **Regular Monitoring**
   - [ ] Check system performance metrics
   - [ ] Monitor error logs
   - [ ] Verify process completion rates
   - [ ] Check resource utilization

3. **Evening Procedures**
   - [ ] Clean up completed processes
   - [ ] Optimize databases
   - [ ] Back up critical data
   - [ ] Generate daily performance report

## Emergency Procedures

1. **System Failure Recovery**

   ```python
   async def emergency_recovery():
       # Stop all active processes
       await halt_active_processes()
       
       # Save system state
       await save_system_state()
       
       # Reinitialize core components
       await reinitialize_core_systems()
       
       # Restore from last known good state
       await restore_system_state()
   ```

2. **Data Integrity Issues**

   ```python
   async def recover_data_integrity():
       # Verify database integrity
       await verify_database_integrity()
       
       # Check process consistency
       await verify_process_consistency()
       
       # Repair corrupted data
       await repair_corrupted_data()
   ```

3. **Process Deadlock Resolution**

   ```python
   async def resolve_deadlocks():
       # Identify deadlocked processes
       deadlocks = await find_deadlocked_processes()
       
       # Break deadlocks
       await resolve_process_deadlocks(deadlocks)
       
       # Verify system stability
       await verify_system_stability()
   ```

For any questions or additional operational needs, please contact the system administrator.
