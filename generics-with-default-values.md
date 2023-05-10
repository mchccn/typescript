## Generic with default values

This is written mainly to explain why an error occurs, and addresses multiple ways to correct this later.
If you want to fix it first, skip ahead.
With that out of the way, here is the minimal reproducible example:

```ts
function foo<T extends string>(arg: T = "") {}
//                             ~~~~~~~~~~~
```

And we get the following somewhat cryptic error message:

> ```
> Type 'string' is not assignable to type 'T'.
>     'string' is assignable to the constraint of type 'T', but 'T' could be instantiated with a different subtype of constraint 'string'.
> ```

For the new user of generics, this is quite confusing.
"`string` is assignable to the constraint of type `T`", sure, that makes sense; a `string` is indeed assignable to a `string`. 
But what about the second part? `T` could be instantiated with a different subtype of constraint `string`.
Now that's going to take a while to digest.

Let's break it down, starting with "`T` could be instantiated".
The easiest way to showcase a type being instantiated would be to explicitly pass the type argument:

```ts
foo<string>("hello world");
//  ~~~~~~ T is instantiated to be a `string` type
```

Instantiation of a type is simply the creation of a type by the compiler to use. 
When a type parameter is not passed, it will be inferred.
In this case, 

```ts
foo/*<"hello world">*/("hello world");
//    ~~~~~~~~~~~~~ T is inferred as "hello world", then instantiated
```

Now, `T` is inferred first, and then instantiated.
So what does this all mean for our original error message?
Let's move on to the second part; "with a different subtype of constraint `string`".

Simply put, `T` could be a different type than `string`.
More importantly, it can be *narrower* than `string`, and therefore, an assignment of `string` to `T` is unsound.
But why?

An important idea of generics is that the **caller** gets to choose `T`, not the implementation.
Here is what I mean:

```ts
foo<"not an empty string">();
//  ~~~~~~~~~~~~~~~~~~~~~ T is "not an empty string"
```

We, as the caller of this function, chose what `T` should be just by passing a type argument.
However, if you remember the definition of `foo`, the parameter `arg` is now holding a `""` (the type is `string`, though) since that was the default provided.
Furthermore, the type of `arg` is `T`...

Do you see what just happened?
We assigned something of type `string` to `"not an empty string"`, which should be invalid!
Thankfully, TypeScript has got our backs this time and warned us (the error message is questionable, but I digress).

Alright, so what can we do to fix this? The easy way out would be to assert that the default value is in fact `T` (even though it isn't):

```ts
function foo<T extends string>(arg: T = "" as T) {} // okay... but not safe
```

But this is obviously unsafe...
The original problem of assigning a `string` to something that can be narrower than `string` still persists.
Admittedly, it's unlikely that a user would try call it as `foo<T>()`, but this is just ignoring the problem!

The go-to solution I have used for this kind of problem is to use overloads.
One overload to take care of the generic parameter, and the other to cover the case where the argument is not given.
This is usually enough to fix the issue, but for more nuanced cases where this completely messes your definition up, you might have to resort to an assertion.

```ts
function foo<T extends string>(arg: T): void;
function foo(): void;
function foo(arg = "") {}
```

Hopefully, TypeScript will get better error messages.
Or at the very least, provide a resource on diagnosing the causes of these errors, so I don't have to write one myself. Good luck!
