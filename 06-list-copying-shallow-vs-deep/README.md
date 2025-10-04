# Step 6: Understanding List Copying - Assignment, Shallow Copy, and Deep Copy

## The Three Ways to "Copy" a List

When working with lists in Python, there are three different ways to copy them:
1. **Assignment** (`=`) - Creates an alias (not really a copy!)
2. **Shallow Copy** (`copy.copy()`) - Copies the outer list but shares nested objects
3. **Deep Copy** (`copy.deepcopy()`) - Copies everything including nested objects

Understanding the differences is crucial for avoiding unexpected behavior in your programs.

## 1. Assignment (`=`) - Creating an Alias

When you use `=`, you're **not creating a copy**. Instead, you're creating an **alias** - two variables pointing to the same list object.

```python
import copy

# Original list
original_list = [1, 2, 3, 4, 5]
print(f"Original list: {original_list}")
print(f"Original list ID: {id(original_list)}")

# Assignment creates an alias, not a copy
assigned_list = original_list
print(f"Assigned list: {assigned_list}")
print(f"Assigned list ID: {id(assigned_list)}")

# Notice that both variables have the SAME ID!
print(f"Do they have the same ID? {id(original_list) == id(assigned_list)}")  # True

# Modifying through one variable affects both
assigned_list.append(99)
print(f"\nAfter modifying assigned list:")
print(f"Original list: {original_list}")    # [1, 2, 3, 4, 5, 99] - changed!
print(f"Assigned list: {assigned_list}")    # [1, 2, 3, 4, 5, 99] - changed!
```

### Assignment with Nested Lists:
```python
# Original nested list
original_nested = [[1, 2], [3, 4], [5, 6]]
print(f"Original nested list: {original_nested}")
print(f"Original nested list ID: {id(original_nested)}")
print(f"Original nested list[0] ID: {id(original_nested[0])}")

# Assignment
assigned_nested = original_nested
print(f"Assigned nested list ID: {id(assigned_nested)}")
print(f"Assigned nested list[0] ID: {id(assigned_nested[0])}")

# Both have the same IDs!
assigned_nested[0].append(99)
print(f"\nAfter modifying assigned nested list[0]:")
print(f"Original nested list: {original_nested}")    # [[1, 2, 99], [3, 4], [5, 6]] - changed!
print(f"Assigned nested list: {assigned_nested}")    # [[1, 2, 99], [3, 4], [5, 6]] - changed!
```

## 2. Shallow Copy (`copy.copy()`) - Copying the Outer Container

A shallow copy creates a new list, but the **elements inside are still references** to the same objects as the original.

```python
import copy

# Original list
original_list = [1, 2, 3, 4, 5]
print(f"Original list: {original_list}")
print(f"Original list ID: {id(original_list)}")

# Shallow copy creates a new list object
shallow_copied = copy.copy(original_list)
print(f"Shallow copied list: {shallow_copied}")
print(f"Shallow copied list ID: {id(shallow_copied)}")

# Now they have different IDs!
print(f"Do they have the same ID? {id(original_list) == id(shallow_copied)}")  # False

# Modifying the shallow copy doesn't affect the original
shallow_copied.append(99)
print(f"\nAfter modifying shallow copy:")
print(f"Original list: {original_list}")     # [1, 2, 3, 4, 5] - unchanged!
print(f"Shallow copied list: {shallow_copied}")  # [1, 2, 3, 4, 5, 99] - changed!
```

### Shallow Copy with Nested Lists (Important!):
```python
import copy

# Original nested list
original_nested = [[1, 2], [3, 4], [5, 6]]
print(f"Original nested list: {original_nested}")
print(f"Original nested list ID: {id(original_nested)}")
print(f"Original nested list[0] ID: {id(original_nested[0])}")

# Shallow copy
shallow_copy = copy.copy(original_nested)
print(f"Shallow copied list ID: {id(shallow_copy)}")
print(f"Shallow copied list[0] ID: {id(shallow_copy[0])}")

# The outer list has different ID, but nested elements have the same IDs!
print(f"Outer lists have same ID? {id(original_nested) == id(shallow_copy)}")  # False
print(f"Inner lists have same ID? {id(original_nested[0]) == id(shallow_copy[0])}")  # True

# Modifying the outer level doesn't affect each other
shallow_copy.append([7, 8])
print(f"\nAfter adding a new sublist:")
print(f"Original: {original_nested}")       # [[1, 2], [3, 4], [5, 6]] - unchanged
print(f"Shallow copy: {shallow_copy}")      # [[1, 2], [3, 4], [5, 6], [7, 8]]

# BUT modifying a nested element affects both!
shallow_copy[0].append(99)
print(f"\nAfter modifying nested element:")
print(f"Original: {original_nested}")       # [[1, 2, 99], [3, 4], [5, 6]] - changed!
print(f"Shallow copy: {shallow_copy}")      # [[1, 2, 99], [3, 4], [5, 6], [7, 8]] - also changed!
```

## 3. Deep Copy (`copy.deepcopy()`) - Complete Copy

A deep copy creates a new list and **recursively copies all nested objects**, creating completely independent copies.

```python
import copy

# Original nested list
original_nested = [[1, 2], [3, 4], [5, 6]]
print(f"Original nested list: {original_nested}")
print(f"Original nested list ID: {id(original_nested)}")
print(f"Original nested list[0] ID: {id(original_nested[0])}")

# Deep copy
deep_copied = copy.deepcopy(original_nested)
print(f"Deep copied list ID: {id(deep_copied)}")
print(f"Deep copied list[0] ID: {id(deep_copied[0])}")

# Both outer and inner lists have different IDs!
print(f"Outer lists have same ID? {id(original_nested) == id(deep_copied)}")     # False
print(f"Inner lists have same ID? {id(original_nested[0]) == id(deep_copied[0])}")  # False

# Now modifications to nested elements don't affect each other
deep_copied[0].append(99)
print(f"\nAfter modifying nested element in deep copy:")
print(f"Original: {original_nested}")      # [[1, 2], [3, 4], [5, 6]] - unchanged!
print(f"Deep copy: {deep_copied}")         # [[1, 2, 99], [3, 4], [5, 6]] - only deep copy changed!
```

## Visual Comparison

Let's see all three methods side by side:

```python
import copy

# Original nested list
original = [[1, 2], [3, 4], [5, 6]]
print("Original:", original, "- ID:", id(original), "- Sublist ID:", id(original[0]))

# Assignment (alias)
assigned = original
print("Assigned:", assigned, "- ID:", id(assigned), "- Sublist ID:", id(assigned[0]))

# Shallow copy
shallow = copy.copy(original)
print("Shallow: ", shallow, "- ID:", id(shallow), "- Sublist ID:", id(shallow[0]))

# Deep copy
deep = copy.deepcopy(original)
print("Deep:    ", deep, "- ID:", id(deep), "- Sublist ID:", id(deep[0]))

# Now let's make changes to see the differences:
deep[0].append(99)      # Modify deep copy
shallow[1].append(88)   # Modify shallow copy
assigned[2].append(77)  # Modify assigned (affects original too)

print("\nAfter various modifications:")
print("Original:", original)    # Affected by assigned modification: [[1, 2], [3, 4, 88], [5, 6, 77]]
print("Assigned:", assigned)    # Same as original: [[1, 2], [3, 4, 88], [5, 6, 77]]
print("Shallow: ", shallow)     # [1] affected by shallow mod, [2] by assigned mod: [[1, 2], [3, 4, 88], [5, 6, 77]]
print("Deep:    ", deep)        # Unchanged by other modifications: [[1, 2, 99], [3, 4], [5, 6]]
```

## When to Use Each Method

### Use Assignment (`=`) when:
- You want two variables pointing to the same list (rarely what you want for "copying")

### Use Shallow Copy (`copy.copy()`) when:
- Your list contains only immutable objects (numbers, strings)
- You want to copy the outer container but keep references to the same nested objects
- You're working with simple nested structures and performance is important

### Use Deep Copy (`copy.deepcopy()`) when:
- Your list contains mutable objects that you want to copy independently
- You want completely independent copies
- You're working with complex nested structures

## Summary

| Method | Creates New Outer Object | Creates New Nested Objects | Use When |
|--------|--------------------------|----------------------------|----------|
| Assignment (`=`) | No | No | Want aliases to same object |
| Shallow Copy (`copy.copy()`) | Yes | No | Simple lists or want shared nested objects |
| Deep Copy (`copy.deepcopy()`) | Yes | Yes | Need completely independent copies |

Understanding these differences is crucial for avoiding unexpected behavior in your Python programs!