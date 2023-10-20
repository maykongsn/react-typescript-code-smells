## Many Non-Null Assertions
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