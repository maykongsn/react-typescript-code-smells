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