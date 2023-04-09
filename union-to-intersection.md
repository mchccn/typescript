## UnionToIntersection

```ts
type UnionToIntersection<U> = (U extends any ? (_: U) => void : never) extends (_: infer I) => void ? I : never;
```

...