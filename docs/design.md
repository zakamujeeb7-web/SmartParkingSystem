# Smart Parking System - Design Documentation

## 1. System Architecture

### 1.1 Zone and Slot Representation

**Data Structures:**
- **Zone**: Contains multiple ParkingAreas, tracks adjacent zones
- **ParkingArea**: Container for ParkingSlots within a zone
- **ParkingSlot**: Individual parking space with availability status

**Relationships:**
```
City
 └── Zones (list)
      └── ParkingAreas (list)
           └── ParkingSlots (list)
```

## 2. Allocation Strategy

### 2.1 Algorithm
1. Search requested zone for available slots (O(n))
2. If found, allocate first available
3. If not found, search adjacent zones
4. Apply cross-zone penalty flag
5. Return allocation result

### 2.2 Cross-Zone Rules
- Allowed when requested zone is full
- Penalty/cost flag set to true
- Searches adjacent zones only

## 3. Request Lifecycle State Machine

### 3.1 State Diagram
```
REQUESTED ──→ ALLOCATED ──→ OCCUPIED ──→ RELEASED
    │              │
    └──→ CANCELLED ←┘
```

### 3.2 Valid Transitions
- REQUESTED → {ALLOCATED, CANCELLED}
- ALLOCATED → {OCCUPIED, CANCELLED}
- OCCUPIED → {RELEASED}
- RELEASED → {} (terminal)
- CANCELLED → {} (terminal)

## 4. Rollback Design

### 4.1 Stack-Based Approach
- Operations stored in LIFO stack
- Each entry contains: request_id, slot, previous_state, timestamp
- Rollback pops last k operations
- Restores slot availability and request states

### 4.2 Rollback Operation
```python
for i in range(k):
    operation = stack.pop()
    free_slot(operation.slot)
    restore_request_state(operation.request_id, operation.previous_state)
```

## 5. Complexity Analysis

### 5.1 Time Complexity
- **Slot Allocation**: O(n) - n = slots in zone
- **Cross-Zone Search**: O(z * n) - z = adjacent zones, n = slots per zone
- **Rollback**: O(k) - k = operations to undo
- **Analytics**: O(r) - r = total requests

### 5.2 Space Complexity
- **City Storage**: O(z * a * s) - z=zones, a=areas, s=slots
- **Request History**: O(r) - r = total requests
- **Rollback Stack**: O(k) - k = max stack size

## 6. Analytics Queries

### 6.1 Implemented Metrics
1. **Average Parking Duration**: Sum(end_time - start_time) / completed_count
2. **Zone Utilization**: (occupied_slots / total_slots) * 100
3. **Request Status Distribution**: Count by state
4. **Peak Usage Zones**: Zone with highest occupation rate
