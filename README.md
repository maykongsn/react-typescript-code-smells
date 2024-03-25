# React and TypeScript Code Smells

> Identifying and analyzing the most common code smells in development with React and TypeScript.

---

## Summary

[Code smells](#code-smells) <br>
  - [TypeScript smells](#typescript-smells)
    - [Any Type](#any-type)
    - [Non-Null Assertions](#non-null-assertions)
    - [Missing Union Type Abstraction](#missing-union-type-abstraction)
    - [Enum Implicit Values](#enum-implicit-values)
  - [React and TypeScript](#react-and-typescript-smells)
    - [Multiple Booleans for State](#multiple-booleans-for-state)
    - [Children Props Pitfall](#children-props-pitfall)
---

## Code Smells

### TypeScript smells

#### Any Type
TypeScript allows type checking and type inference for variables, objects, functions, etc. By defining the type ``any`` for an element in the code, the developer is disabling type checking, allowing manipulation of these entities without any verification. For example, consider the code below, where a state has its type defined as ``any``. This is problematic because it nullifies the benefits that TypeScript offers in terms of type safety and compile-time error detection. Consequently, without the check, runtime errors may occur.

```tsx
import React, { useState } from 'react';

const MyComponent = () => {
  const [data, setData] = useState<any>(null);

  // ...
};

export default MyComponent;
```

---

## Non-Null Assertions
It is common for developers to use the non-null operator to indicate that a property will not be null or undefined in cases where type checking cannot deduce this. In the following example, the interface ``DataProps`` infers that ``data`` can be of type null. For this reason, the developer uses the non-null operator to assure TypeScript that ``data`` will not be null. Consequently, runtime errors may occur due to lack of type checking and the possibility of receiving null.

```tsx
function Component({ data }: { data: { prop1: string; prop2: number } | null }) {
  return(
      <div>
        <p>{data!.prop1}</p>
        <p>{data!.prop2}</p>
      </div>
  )
}
```

---

## Missing Union Type Abstraction
``Type aliases`` in TypeScript are similar to interfaces. However, they accept ``union types``, something interfaces do not allow. Additionally, they facilitate code maintenance, reusability, and code readability. The following example demonstrates a component where the props are of type string, number, or boolean. In this case, it is recommended to use type aliases to avoid repetition of union types.

```tsx
function Component({ prop }: {
  prop: string | number | boolean;
}) {
  return (
    // ...
  );
}
```

---

## Enum Implicit Values
Enums are a way to define constants and facilitate code maintenance and readability. However, in TypeScript, it is common for developers to use enums without defining explicit values for them. Therefore, TypeScript assigns numerical values to them following the order. In this example, the constants ``Pending``, ``Processing`` and ``Completed`` are without explicit values. Consequently, if the developer adds a new constant like ``Failed``, it may change the order and the numerical values of the enum.

```tsx
enum PaymentStatus {
  Pending,
  Processing,
  Completed,
}
```

---

### React and TypeScript smells

## Multiple Booleans for State
Developers commonly use many boolean-type states to define the component's state. This increases complexity and makes the code difficult to maintain as the number of states and typing increases. In the example shown, the component has some ``useState`` of boolean type, which can be replaced with better typing alternatives.

```tsx
function Component() {
    const [isActive, setIsActive] = useState(false);
    const [isLoading, setIsLoading] = useState(false);
    const [isVisible, setIsVisible] = useState(false);
    // ...
}
```

---

## Children Props Pitfall
If the component uses children props, developers should opt for the correct typing to be able to pass JSX elements. Therefore, developers should use ``ReactNode`` or some other primitive type, functions, etc. For this reason, it is not a good practice to define ``any`` or types that can restrict JSX elements and affect readability, such as: ``undefined``, ``null``, ``unknown`` and ``never``.

```tsx
type ComponentProps = {
  children: undefined;
};

interface AnotherComponentProps {
  children: never;
}
```

---