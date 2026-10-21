# Code Smells and Refactoring

## Group Members

- Qiaochu Liu
- Alice Gu

## Selected Code Smells

- Long Method
- Duplicated Code
## Duplicated Code

### Introduction

Duplicated code occurs when the same or very similar logic appears in multiple places in a program. It often happens when developers copy and paste code instead of reusing existing functionality.

For example, two functions might calculate the same discount for different products.

### Why Is It a Problem?

- **More maintenance:** The same change must be made in multiple places.
- **Inconsistent behavior:** A developer might update one copy but forget another.
- **Repeated bugs:** A mistake in the duplicated logic can affect several parts of the program.

### Prevention and Refactoring

Before writing new code, check whether the same logic already exists.

One way to remove duplicated code is Fowler’s **Extract Function** refactoring. Move the repeated logic into a shared function, then call that function wherever it is needed.

Combine code when it represents the same rule. Similar-looking code may serve different purposes and may need to stay separate.

### Example 1: Python

Both functions below apply the same 10% discount.

#### Before Refactoring

```python
def book_price(price):
    return price * 0.9

def shirt_price(price):
    return price * 0.9

print(book_price(50))   # 45.0
print(shirt_price(20))  # 18.0
```

The discount calculation is duplicated. If the shared discount changes, both functions must be updated.

#### After Refactoring

```python
def apply_discount(price):
    return price * 0.9

def book_price(price):
    return apply_discount(price)

def shirt_price(price):
    return apply_discount(price)

print(book_price(50))   # 45.0
print(shirt_price(20))  # 18.0
```

Using **Extract Function**, we moved the calculation into `apply_discount()`. Both product functions now use the same implementation. The results remain the same, and future changes to the shared discount require editing only one place.

### Example 2: JavaScript

This example uses the same discount rule in JavaScript.

#### Before Refactoring

```javascript
function bookPrice(price) {
    return price * 0.9;
}

function shirtPrice(price) {
    return price * 0.9;
}

console.log(bookPrice(50));  // 45
console.log(shirtPrice(20)); // 18
```

Both functions repeat the same calculation, creating the same maintenance problem as the Python example.

#### After Refactoring

```javascript
function applyDiscount(price) {
    return price * 0.9;
}

function bookPrice(price) {
    return applyDiscount(price);
}

function shirtPrice(price) {
    return applyDiscount(price);
}

console.log(bookPrice(50));  // 45
console.log(shirtPrice(20)); // 18
```

The shared function removes the duplicated calculation without changing the results.

These examples assume books and shirts follow the same discount policy. If their discount policies become independent, separate implementations may be appropriate.

### References

- Martin Fowler, *Refactoring: Improving the Design of Existing Code*, 2nd edition, Chapter 3: “Bad Smells in Code,” Duplicated Code.
- [Extract Function — Martin Fowler’s Refactoring Catalog](https://refactoring.com/catalog/extractFunction.html)
