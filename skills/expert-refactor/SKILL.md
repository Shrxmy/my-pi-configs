---
name: "expert-refactor"
description: "Expert code refactoring specialist focused on improving code quality, reducing technical debt, and applying clean code principles. Use when refactoring legacy code, improving code structure, applying design patterns, or reducing code complexity. Covers SOLID principles, design patterns, extract methods, and safe transformation techniques."
compatibility: pi
---

# Expert Refactor

Mastery of systematic code refactoring while preserving functionality.

## Core Principles

### 1. Small Changes

Make incremental changes that can be easily tested and rolled back.

```python
# Before: Long function
def process_user_data(user_data):
    # 50 lines of mixed logic
    return result

# After: Extracted small functions
def process_user_data(user_data):
    validated = validate_user(user_data)
    enriched = enrich_user_data(validated)
    return save_user(enriched)
```

### 2. Rename Refactoring

Use IDE refactoring tools for safe renaming.

```python
# Rename variable
old_name = "user"  # → new_name = "customer"

# Rename function
def calculate_total():  # → def compute_order_total():
    pass
```

### 3. Extract Method

```python
# Before
def create_report(self):
    self.fetch_data()
    self.format_data()  
    self.validate_data()
    self.save_report()
    self.notify_user()

# After: Each method is testable
def create_report(self):
    data = self.fetch_data()
    formatted = self.format_data(data)
    validated = self.validate_data(formatted)
    self.save_report(validated)
    self.notify_user()
```

### 4. Replace Conditional with Polymorphism

```python
# Before: Complex if/else
def calculate_shipping(order):
    if order.is_international:
        if order.weight > 10:
            return 25.00
        else:
            return 15.00
    else:
        return 5.00

# After: Strategy pattern
class ShippingCalculator:
    def calculate(self, order):
        strategy = self.get_strategy(order)
        return strategy.calculate(order)
```

### 5. Introduce Parameter Object

```python
# Before: Too many parameters
def create_user(name, email, phone, address, age, preferences):
    pass

# After: Grouped parameters
@dataclass
class UserProfile:
    name: str
    email: str
    phone: str
    address: str
    age: int
    preferences: dict

def create_user(profile: UserProfile):
    pass
```

### 6. Replace Magic Numbers

```python
# Before
if status_code == 200:
    return "success"

# After
class StatusCode:
    SUCCESS = 200
    ERROR = 500
    
if status_code == StatusCode.SUCCESS:
    return "success"
```

## Refactoring Patterns

### Code Smells to Fix

| Smell | Solution |
|-------|----------|
| Long method | Extract method |
| Duplicate code | Abstract to shared function |
| God class | Split into focused classes |
| Feature envy | Move method to related class |
| Data clumps | Introduce parameter object |
| Primitive obsession | Use value objects |
| Switch statements | Use polymorphism |
| Lazy class | Remove or inline |

### SOLID Principles

1. **S**ingle Responsibility: One reason to change
2. **O**pen/Closed: Open for extension, closed for modification
3. **L**iskov Substitution: Subtypes must be substitutable
4. **I**nterface Segregation: Many specific interfaces
5. **D**ependency Inversion: Depend on abstractions

## Safe Refactoring Steps

1. **Pin Behavior**: Read project conventions and tests; identify externally visible behavior that must remain unchanged
2. **Find the Seam**: Prefer improving a cohesive module boundary or making a deep module simpler to use over cosmetic reshuffling
3. **Cover**: Add or update focused tests before refactoring when practical; keep feature changes distinct from structure-only changes
4. **Refactor in Slices**: Make one behavior-preserving change at a time and run the narrowest relevant checks
5. **Review Scope**: Inspect the final diff for accidental behavior, UI, or file-structure changes; never commit unless requested

## Tools

```bash
# Code complexity analysis
pip install radon

# Analyze complexity
radon cc src/ -a

# Analyze maintainability
radon mi src/ -a
```

## Quick Reference

| Technique | When to Use |
|-----------|-------------|
| Extract Method | Function > 10 lines |
| Rename | Unclear naming |
| Move Method | Method uses another class more |
| Introduce Parameter Object | 4+ parameters on method |
| Replace Conditional | Complex if/else chains |
| Replace Temp with Query | Temporary variables |
| Introduce Null Object | Null checks everywhere |
| Extract Class | Too many responsibilities |