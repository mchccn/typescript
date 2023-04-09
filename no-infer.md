## NoInfer

```ts
type NoInfer<T> = [T][T extends any ? 0 : never];
```

This type is used to block the compiler from using an inference site.
For example, given the following function `inference`:

```ts
declare function inference<T>(a: T, b: T);

inference("", 0);
//            ~ Argument of type 'number' is not assignable to parameter of type 'string'.
```

`T` is inferred as `string`, because of the first argument.
If you want `T` to be inferred from the second parameter instead, you would have to block the compiler's inference in the parameter `a`.
This is where `NoInfer` can be used:

```ts
declare function inference<T>(a: NoInfer<T>, b: T);

inference("", 0);
//        ~~ Argument of type 'string' is not assignable to parameter of type 'number'.
```

Now, `T` is inferred as `number`, as the compiler could not use the first argument as a point of inference.
Since `T` is `number`, the first argument now generates an error saying that a string cannot be assigned to a number.
