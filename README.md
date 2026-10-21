# Code Smells and Refactoring

## Group Members

- Qiaochu Liu
- Alice Gu

## Selected Code Smells

- Long Method
- Duplicated Code

## Long Method

### Introduction

A **Long Method** is a method or function that contains too much code or performs too many tasks. It often happens when developers continue adding new functionality to an existing function instead of dividing the work into smaller, more focused functions.

For example, a checkout function might calculate the total price, apply discounts, calculate taxes, and print a receipt all in one place.

### Why Is It a Problem?

- **Harder to understand:** A long function can be difficult to read and understand.
- **Difficult to maintain:** Changing one part of the function may affect other parts.
- **Harder to test:** A function that performs many tasks can be difficult to test independently.
- **Code duplication:** Developers may copy parts of a long function instead of extracting reusable logic.

### Prevention and Refactoring

Keep functions focused on a single responsibility. When a function becomes too long or contains several distinct steps, consider extracting those steps into smaller functions with clear names.

One of Fowler's refactoring patterns is **Extract Function**. It moves a group of statements into a new function and replaces those statements with a function call.

However, function length alone does not determine whether a method is problematic. The main goal is to make the code easier to understand and maintain.

### Example 1: Python

This example calculates the final price of an order and prints a receipt.

#### Before Refactoring

```python
def checkout(price, quantity):
    subtotal = price * quantity
    discount = 0

    if subtotal >= 100:
        discount = subtotal * 0.1

    discounted_total = subtotal - discount
    tax = discounted_total * 0.08
    final_total = discounted_total + tax

    print("Subtotal:", subtotal)
    print("Discount:", discount)
    print("Tax:", tax)
    print("Final total:", final_total)

    return final_total

checkout(60, 2)
```

The `checkout()` function calculates the subtotal, applies a discount, calculates tax, prints the receipt, and returns the final price. Having all these steps in one function makes it harder to understand and modify individual parts.

#### After Refactoring

```python
def calculate_subtotal(price, quantity):
    return price * quantity


def apply_discount(subtotal):
    if subtotal >= 100:
        return subtotal * 0.9
    return subtotal


def calculate_tax(amount):
    return amount * 0.08


def print_receipt(subtotal, discount, tax, final_total):
    print("Subtotal:", subtotal)
    print("Discount:", discount)
    print("Tax:", tax)
    print("Final total:", final_total)


def checkout(price, quantity):
    subtotal = calculate_subtotal(price, quantity)
    discounted_total = apply_discount(subtotal)
    discount = subtotal - discounted_total
    tax = calculate_tax(discounted_total)
    final_total = discounted_total + tax

    print_receipt(subtotal, discount, tax, final_total)
    return final_total


checkout(60, 2)
```

Using **Extract Function**, we divided the checkout process into smaller functions, each with a clear purpose. The `checkout()` function now describes the overall process, while the details are handled by separate functions.

The calculation and output remain the same, but the code is easier to read, test, and maintain.

### Example 2: JavaScript

This example performs the same checkout process using JavaScript.

#### Before Refactoring

```javascript
function checkout(price, quantity) {
    const subtotal = price * quantity;
    let discount = 0;

    if (subtotal >= 100) {
        discount = subtotal * 0.1;
    }

    const discountedTotal = subtotal - discount;
    const tax = discountedTotal * 0.08;
    const finalTotal = discountedTotal + tax;

    console.log("Subtotal:", subtotal);
    console.log("Discount:", discount);
    console.log("Tax:", tax);
    console.log("Final total:", finalTotal);

    return finalTotal;
}

checkout(60, 2);
```

The function handles several different tasks, making it longer and harder to maintain.

#### After Refactoring

```javascript
function calculateSubtotal(price, quantity) {
    return price * quantity;
}

function applyDiscount(subtotal) {
    return subtotal >= 100 ? subtotal * 0.9 : subtotal;
}

function calculateTax(amount) {
    return amount * 0.08;
}

function printReceipt(subtotal, discount, tax, finalTotal) {
    console.log("Subtotal:", subtotal);
    console.log("Discount:", discount);
    console.log("Tax:", tax);
    console.log("Final total:", finalTotal);
}

function checkout(price, quantity) {
    const subtotal = calculateSubtotal(price, quantity);
    const discountedTotal = applyDiscount(subtotal);
    const discount = subtotal - discountedTotal;
    const tax = calculateTax(discountedTotal);
    const finalTotal = discountedTotal + tax;

    printReceipt(subtotal, discount, tax, finalTotal);
    return finalTotal;
}

checkout(60, 2);
```

The refactored version separates the calculations and receipt printing into smaller functions. The main `checkout()` function is now easier to follow, and each helper function can be tested independently.

These examples use the same pricing rules before and after refactoring, so the final result remains unchanged.

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
