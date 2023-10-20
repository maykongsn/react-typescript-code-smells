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