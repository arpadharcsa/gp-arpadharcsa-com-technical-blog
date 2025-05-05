---
title: " Escaping Primitive Obsession"
description: "Primitive Obsession is a common anti-pattern in low level software design where primitive data types are used excessively instead of creating custom types that better represent the domain. 
This can lead to code that is difficult to maintain and understand."
date: 2025-05-05 08:00:00 +/-TTTT
categories: [code-smells, readability, maintainability]
tags: [software-design]     # TAG names should always be lowercase
image:
  path: /assets/img/codesmells.jpeg
---

# What is Primitive Obsession?

Primitive Obsession occurs when developers use basic data types (like `int`, `String`, `boolean`) to represent complex concepts. 
While primitives are convenient, they often fail to encapsulate the domain logic, leading to scattered validation and business rules throughout the codebase.

# Example

Let's consider a scenario where we need to manage email addresses in an application 
and we want to ensure that the email addresses are valid and consistent throughout the system.

## Primitive approach
A naive approach might use a `String` to represent email address:

```java
public class User {
  private String email;

  public User(String email) {
    this.email = email;
  }

  public String getEmail() {
    return email;
  }
}
```
In this example, the String type doesn't enforce any rules or validations which can lead to issues like invalid formats or inconsistent data handling.

Potential drawbacks:
- Validation: There's no built-in mechanism to ensure the `String` is a valid email format.
- Duplication: Validation logic might be duplicated across different parts of the codebase.
- Expressiveness: A `String` does not able to express the semantics of an email address which is leading to unreadable code.
- Potential Bugs: Easy to assign non-email strings to the email field which is also a risk.

## Value Object approach

Instead of using a primitive type, we can create a dedicated class to represent an email address.

```java
public class Email {
  private final String address;

  public Email(String address) {
    if (!isValidEmail(address)) {
      throw new IllegalArgumentException("Invalid email address");
    }
    this.address = address;
  }

  public String getAddress() {
    return address;
  }

  private boolean isValidEmail(String email) {
    // Use a regex or a library to validate the email format
    return email.matches("^[A-Za-z0-9+_.-]+@[A-Za-z0-9.-]+$");
  }

  @Override
  public boolean equals(Object o) {
    if (this == o) return true;
    if (o == null || getClass() != o.getClass()) return false;
    Email email = (Email) o;
    return address.equals(email.address);
  }

  @Override
  public int hashCode() {
    return address.hashCode();
  }

  @Override
  public String toString() {
    return address;
  }
}

```

Benefits of using a value object:
- **Readability:** This class clearly represents an email address, improving code readability.
- **Maintainability:** Also encapsulates validation and formatting logic, making it easier to manage and update.
- **Immutability:** By making the `Email` object immutable, you prevent accidental changes to the email address once it's created.

# Takeaway
Transforming primitive types into value objects can be a powerful technique to enhance code quality by providing clear abstraction over domain concepts.
This approach not only improves readability and maintainability but also helps in enforcing business rules and validations on a consistent way.
