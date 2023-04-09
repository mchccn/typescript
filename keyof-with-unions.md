## `keyof` with unions

Yet another common gotcha I often see is the use of `keyof` on unions, expecting the result to be the keys of all members in the union. 
Here is an example to demonstrate what I mean:

```ts
type A = { foo: string; bar: number };
type B = { bar: boolean; baz: number };

type Keys = keyof (A | B); // I want this to be "foo" | "bar" | "baz"
```

Unfortunately, `Keys` is actually only `"bar"`. In other words, `keyof T` where `T` is a union actually only gets the keys that are shared with all members of `T`.
Many people get caught out by this and they always ask, "Why is that so?", or outright say, "That doesn't make sense".
I believe most of you have encountered a situation like the following before (using the same types as above):

```ts
declare const foo: A | B;

foo.bar
//  ~~~ Suggests only "bar"

foo.foo // Property 'foo' does not exist on type 'B'.
```

You can only access the keys that are shared with all members of the union, which should make sense, as accessing a key that is not shared may not be defined and therefore it is unsafe/unsound.
This is the same for the `keyof` operator. It only gets the shared keys because those are the only keys that are safe to use on the union.

If you need to get "all" keys of a union, you can either do it individually:

```ts
type Keys = keyof A | keyof B; // "foo" | "bar" | "baz"
```

Or you can use `keyof` on the **intersection** of the members in the union:

```ts
type Keys = keyof (A & B);
```

This works since all keys of the intersection are "safe" to use.
If you don't want to do all of this manually, you can use [UnionToIntersection](union-to-intersection.md) to transform the union into an intersection for you:

```ts
type Keys = keyof UnionToIntersection<A | B>;
```

Similarly, you can also a utility like [KeysOfUnion](keys-of-union.md) to get all keys of the union by distributing over the union first:

```ts
type Keys = KeysOfUnion<A | B>;
```

So now, you've pretty much got four methods of correctly getting all keys of a union, shared or not.
You can click the links above to learn more about how the utilities `UnionToIntersection` and `KeysOfUnion` work, but this concludes this note on `keyof` with unions.
