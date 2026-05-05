---
title: "Understanding Callbacks in Dart"
description: "Learn what callback functions are, why they exist, and how to use them in Dart."
---

You already know what a function looks like.

```dart
void greet(String name) {
  print('Hello, $name!');
}

greet('Alice'); // Hello, Alice!
```

It has a name. It has parameters. You call it by name.

But Dart has another kind of function: one without a name.

## Anonymous functions

```dart
(String name) {
  print('Hello, $name!');
};
```

That is an anonymous function. No name. Which raises an obvious question: how do you call it?

You cannot. Not directly. There is nothing to call it by.

Think about how values work. The string `'Alice'` exists, but you cannot do anything useful with it unless you store it somewhere:

```dart
var name = 'Alice';
print(name); // Alice
```

Without the variable, you have no way to reach the value. A function body works the same way. Give it a name by assigning it to a variable:

```dart
Function greet = (String name) {
  print('Hello, $name!');
};

greet('Bob'); // Hello, Bob!
```

You are treating the function body as a value. Assign it, and you can call it by the variable name.

| Variable | Type | Value |
|---|---|---|
| `name` | `String` | `'Alice'` |
| `greet` | `Function` | the anonymous function body |

## Passing a function as an argument

Here is where it gets interesting.

Just like you can pass a `String` or an `int` to a function, you can pass a function too:

```dart
void greeter(Function callback) {
  callback(); // calls whatever function was passed in
}

var greet = () {
  print('Hello, Alice!');
};

greeter(greet); // Hello, Alice!
```

`greeter` takes one parameter, `callback`, of type `Function`. When you call `greeter(greet)`, the anonymous function stored in `greet` is assigned to `callback`. Inside `greeter`, calling `callback()` runs it.

A function passed as an argument like this is called a **callback**.

| Callback | Noun. A function passed as an argument to another function, to be called inside it. |
|---|---|

## Why callbacks matter

Here is a concrete example.

You have a list of numbers and you want to add them all up:

```dart
final List<int> numbers = [1, 2, 3, 4];

int total = 0;
for (int element in numbers) {
  total = total + element;
}
print(total); // 10
```

Now you want to multiply them instead:

```dart
int total = 1;
for (int element in numbers) {
  total = total * element;
}
print(total); // 24
```

The structure is identical. Only two things change: the starting value and the operation. Repeating nearly the same code twice is a sign you can extract the difference.

Make the operation a callback:

```dart
int calculate(List<int> list, int initialValue, Function operation) {
  int total = initialValue;
  for (int element in list) {
    total = operation(total, element); // call the operation on each element
  }
  return total;
}
```

Now define each operation as its own function:

```dart
int sum(int a, int b) {
  return a + b;
}

int multiply(int a, int b) {
  return a * b;
}
```

Pass whichever you need:

```dart
final List<int> numbers = [1, 2, 3, 4];

print(calculate(numbers, 0, sum));      // 10
print(calculate(numbers, 1, multiply)); // 24
```

Same loop. Different behavior. No repeated code.

That is the point of callbacks: they let you pass behavior into a function, not just data.

## Summary

| Concept | What it is |
|---|---|
| Anonymous function | A function without a name, used as a value |
| Function variable | A variable that holds an anonymous function |
| Callback | A function passed as an argument to another function |
