# Smart Parking Allocation & Zone Management System

## Overview
A Python-based smart parking management system with zone-based allocation, state machine request lifecycle, rollback support, and analytics.

## Features
- Multi-zone parking allocation
- Cross-zone allocation with penalty
- Request lifecycle state machine (REQUESTED → ALLOCATED → OCCUPIED → RELEASED)
- Rollback support for last k operations
- Real-time analytics and reporting
- GUI interface using Tkinter

## Setup
```bash
python setup.py  # Create project structure
python main.py   # Run application
```

## Testing
```bash
python -m pytest tests/
```

## Project Structure
```
smart-parking-system/
├── src/           # Core system modules
├── tests/         # Test suite
├── docs/          # Documentation
└── main.py        # Entry point
```

## Requirements
- Python 3.8+
- Tkinter (built-in)

## Development Team
[Your names here]
