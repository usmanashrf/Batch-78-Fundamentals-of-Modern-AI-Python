# Step 5: Understanding Mutable and Immutable Datatypes in Python

## What are Mutable and Immutable Objects?

In Python, **mutable** objects can be changed after they are created, while **immutable** objects cannot be changed once created.

Think of it like this:
- Immutable objects are like a finished sculpture - you can't change it without creating a new one
- Mutable objects are like clay - you can reshape them without making a new piece

## Immutable Objects in Python

Immutable objects **cannot** be changed after creation. When you try to modify them, Python actually creates a new object.

### Common Immutable Types:
- Numbers (`int`, `float`, `complex`)
- Strings
- Tuples
- Frozen sets
- Boolean values

### Example with Strings:

```python
# Strings are immutable
name = "Alice"
print(f"Original name: {name}")  # Output: Original name: Alice

# This doesn't modify the original string, but creates a new one
name = name + " Smith"
print(f"Modified name: {name}")  # Output: Modified name: Alice Smith

# The original string "Alice" still exists in memory until garbage collected
```

### Example with Numbers:

```python
# Numbers are immutable
age = 25
print(f"Original age: {age}")    # Output: Original age: 25

age = age + 5  # Creates a new integer object
print(f"New age: {age}")         # Output: New age: 30
```

## Mutable Objects in Python

Mutable objects **can** be changed after they are created. Modifications happen in-place.

### Common Mutable Types:
- Lists
- Dictionaries
- Sets
- User-defined classes (usually)

### Example with Lists:

```python
# Lists are mutable
my_list = [1, 2, 3]
print(f"Original list: {my_list}")  # Output: Original list: [1, 2, 3]

# This modifies the list in-place (no new object created)
my_list.append(4)
print(f"After append: {my_list}")   # Output: After append: [1, 2, 3, 4]

my_list[0] = 100  # This changes the first element
print(f"After change: {my_list}")   # Output: After change: [100, 2, 3, 4]
```

### Example with Dictionaries:

```python
# Dictionaries are mutable
person = {"name": "Bob", "age": 30}
print(f"Original: {person}")        # Output: Original: {'name': 'Bob', 'age': 30}

# These modifications happen in-place
person["age"] = 35
person["city"] = "New York"
print(f"Modified: {person}")        # Output: Modified: {'name': 'Bob', 'age': 35, 'city': 'New York'}
```

## Important Concepts: References and Aliases

When working with mutable objects, understanding references is crucial:

### With Mutable Objects:

```python
# Two variables pointing to the same list object
list1 = [1, 2, 3]
list2 = list1  # list2 points to the same object as list1

print(f"list1: {list1}")  # Output: list1: [1, 2, 3]
print(f"list2: {list2}")  # Output: list2: [1, 2, 3]

# Changing the list through either variable affects both
list1.append(4)
print(f"After append - list1: {list1}")  # Output: After append - list1: [1, 2, 3, 4]
print(f"After append - list2: {list2}")  # Output: After append - list2: [1, 2, 3, 4]
```

### With Immutable Objects:

```python
# Two variables pointing to the same string object initially
str1 = "hello"
str2 = str1
print(f"str1: {str1}")  # Output: str1: hello
print(f"str2: {str2}")  # Output: str2: hello

# Creating a new string doesn't affect the original
str1 = str1 + " world"
print(f"After modification - str1: {str1}")  # Output: After modification - str1: hello world
print(f"After modification - str2: {str2}")  # Output: After modification - str2: hello
```

## Why Does This Matter?

### 1. Performance
- Mutable objects can be more memory efficient when you need to make many changes
- Immutable objects can be safer as they cannot be accidentally changed

### 2. Function Arguments
```python
def modify_list(lst):
    lst.append(99)  # Modifies the original list
    return lst

def modify_string(s):
    s = s + " modified"  # Creates new string, doesn't change original
    return s

# With mutable object
my_numbers = [1, 2, 3]
modify_list(my_numbers)
print(my_numbers)  # Output: [1, 2, 3, 99] - original list was changed!

# With immutable object
my_text = "Hello"
modify_string(my_text)
print(my_text)     # Output: Hello - original string unchanged!
```

### 3. The `id()` Function: Memory Address Tracking

The `id()` function returns the unique identifier (memory address) of an object. This is very helpful to understand mutable vs immutable behavior:

#### With Immutable Objects (Strings):
```python
text = "Hello"
original_id = id(text)
print(f"Original text: {text}, ID: {original_id}")  # Example: ID: 140234567890123

# When we modify the string, Python creates a new object
text = text + " World"
new_id = id(text)
print(f"Modified text: {text}, ID: {new_id}")      # Example: ID: 140234567890456

# Notice that the ID changed, meaning it's a completely new object!
print(f"Did the ID change? {original_id != new_id}")  # Output: Did the ID change? True
```

#### With Mutable Objects (Lists):
```python
my_list = [1, 2, 3]
original_id = id(my_list)
print(f"Original list: {my_list}, ID: {original_id}")  # Example: ID: 140234567890789

# When we modify the list, it changes in-place
my_list.append(4)
current_id = id(my_list)
print(f"Modified list: {my_list}, ID: {current_id}")   # Example: ID: 140234567890789

# Notice that the ID stayed the same, meaning it's still the same object!
print(f"Did the ID change? {original_id == current_id}")  # Output: Did the ID change? True
```

### 4. Dictionary Keys
Only immutable objects can be used as dictionary keys:

```python
# This works - strings are immutable
valid_dict = {"key1": "value1", "key2": "value2"}

# This works - numbers are immutable  
valid_dict2 = {1: "one", 2: "two", 3.14: "pi"}

# This will cause an error - lists are mutable
# invalid_dict = {[1, 2, 3]: "value"}  # TypeError: unhashable type: 'list'

# But this works - tuples are immutable
valid_dict3 = {(1, 2, 3): "value"}
```

## Quick Memory Trick

**I**mmutable = **I**nchangeable
**M**utable = **M**odifiable

## Summary

- **Immutable objects** (strings, numbers, tuples): Cannot be modified after creation
- **Mutable objects** (lists, dictionaries, sets): Can be modified after creation
- Be careful with mutable object references - changes through one variable affect all references
- Understanding this helps with debugging and writing more efficient code

## Practice Exercise

Try running this code to see the difference:

```python
# Immutable example
a = 5
b = a
a = a + 1
print(f"a: {a}, b: {b}")  # What do you think this will print?

# Mutable example  
x = [1, 2, 3]
y = x
x.append(4)
print(f"x: {x}, y: {y}")  # What do you think this will print?
```

Understanding mutable vs immutable objects is fundamental to writing effective Python code!