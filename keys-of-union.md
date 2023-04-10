## KeysOfUnion

```ts
type KeysOfUnion<U> = U extends U ? keyof U : never;
```

See the related article [`keyof` with unions](keyof-with-unions.md) if you haven't already.
This type is a utility for extracting all keys of a union type.
Counterintuitively to some, `keyof (A | B)` does not give you `keyof A | keyof B`, but rather only the keys shared between the members of the union.

Sometimes, you can [convert the union to an intersection](union-to-intersection.md), and then get the keys of that intersection, but this may break in some cases.
[Distributing the union](https://www.typescriptlang.org/docs/handbook/2/conditional-types.html#distributive-conditional-types) and then using `keyof` on each member of the union is much more effective and relible.

Here's an example from the linked article:

```ts
type A = { foo: string; bar: number };
type B = { bar: boolean; baz: number };

type Keys = keyof (A | B); // "bar"
```

Instead, using `KeysOfUnion`:

```ts
type A = { foo: string; bar: number };
type B = { bar: boolean; baz: number };

type Keys = KeysOfUnion<A | B>; // "foo" | "bar" | "baz"
```