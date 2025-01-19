# Task Portal Quick Reference Guide

## Component Hotkeys

### LogicalProcessor Commands
```
LA - Execute Logical Analysis
EI - Engage Intuition
AR - Execute Abductive Reasoning
LI - Apply Logical Inference
```

### IntelliSynth Framework Hotkeys
```
IF1 - Initialize Actor, Evaluator, Self-Reflection
IF2 - Initialize Policy
IF3 - Set Advancement Weights

CA4 - Apply Universal Intelligence
CA5 - Apply Optimization
CA6 - Apply Learning from Rewards
CA7 - Apply Transfer Learning
CA8 - Apply Adaptability
CA9 - Apply Reasoning
CA0 - Apply Evolutionary Intelligence

SF1 - Apply Imprecise Reasoning
SF2 - Apply Natural Language Understanding
SF3 - Apply Neural Activation
SF4 - Apply Uncertainty

AP1 - Evaluate Truth
AP2 - Conduct Scrutiny
AP3 - Implement Improvement
AP4 - Calculate Advancement

OP1 - Start Overall Process
OP2 - Iterate Improvement
OP3 - Evaluate Advancement
OP4 - Finalize Advancement
```

## Common Operations

### 1. System Health Check
```python
async def quick_health_check():
    # Check logical processor
    await execute_hotkey('LA')
    
    # Check IntelliSynth
    await execute_hotkey('IF1')
    
    # Verify advancement
    await execute_hotkey('AP1')
```

### 2. Emergency Recovery
```python
async def quick_recovery():
    # Initialize components
    await execute_hotkey('IF1')
    
    # Apply optimization
    await execute_hotkey('CA5')
    
    # Verify system state
    await execute_hotkey('OP3')
```

### 3. Performance Optimization
```python
async def quick_optimize():
    # Apply Universal Intelligence
    await execute_hotkey('CA4')
    
    # Apply Optimization
    await execute_hotkey('CA5')
    
    # Apply Adaptability
    await execute_hotkey('CA8')
```

## Common Queries

### 1. Check Process Status
```sql
SELECT process_id, 
       json_extract(logical_analysis_state, '$.status') as status,
       confidence_metrics
FROM logical_processor_state
WHERE created_at >= datetime('now', '-1 hour');
```

### 2. Monitor Framework Health
```sql
SELECT framework_id,
       adaptation_metrics,
       json_extract(feedback_state, '$.status') as feedback
FROM meta_process_framework
WHERE adaptation_metrics < 0.8;
```

### 3. Check IntelliSynth Performance
```sql
SELECT operation_id,
       json_extract(advancement_metrics, '$.value') as advancement
FROM intellisynth_operations
WHERE created_at >= datetime('now', '-1 hour');
```

## Meta Framework Quick Guide

### 1. Establish Inquiry
- Define core goal
- Set guiding principles
- Establish essence

### 2. Identify Dimensions
- Break down components
- Map relationships
- Define variables

### 3. Design Frameworks
- Create macro level
- Implement micro level
- Establish feedback loops

### 4. Leverage Constraints
- Define boundaries
- Focus innovation
- Enable emergence

### 5. Monitor Evolution
- Track adaptation
- Measure progress
- Adjust strategies

## Common Error Resolution

### 1. Logical Processing Errors
```python
# Quick logical reset
await execute_hotkey('LA')
await execute_hotkey('EI')
```

### 2. Framework Errors
```python
# Quick framework reset
await execute_hotkey('IF1')
await execute_hotkey('IF2')
```

### 3. IntelliSynth Errors
```python
# Quick IntelliSynth recovery
await execute_hotkey('CA4')
await execute_hotkey('CA5')
```

## Performance Monitoring

### 1. Quick Health Metrics
```sql
SELECT 
    'Logical' as component,
    AVG(confidence_metrics) as health
FROM logical_processor_state
UNION
SELECT 
    'Framework' as component,
    AVG(adaptation_metrics) as health
FROM meta_process_framework;
```

### 2. Quick Performance Check
```python
async def quick_performance_check():
    await execute_hotkey('AP1')  # Truth
    await execute_hotkey('AP2')  # Scrutiny
    await execute_hotkey('AP4')  # Advancement
```

For detailed procedures and advanced operations, refer to the full Implementation and Operations guides.