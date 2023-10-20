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