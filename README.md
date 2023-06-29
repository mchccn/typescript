# TypeScript

These are notes on TypeScript I have curated from my time spent answering Stack Overflow questions, including behavior, canonical issues, workarounds, and anti-patterns.
I often find myself referencing the same documentation and GitHub issues from time to time, so I thought it would be nice if I had them all in one place.

All notes are written using the latest version of TypeScript.

<a href="https://stackoverflow.com/users/18244921"><img src="https://stackoverflow.com/users/flair/18244921.png" width="208" height="58" alt="profile for vr. at Stack Overflow, Q&amp;A for professional and enthusiast programmers" title="profile for vr. at Stack Overflow, Q&amp;A for professional and enthusiast programmers"></a>

## Code Snippets

This section contains useful TypeScript snippets, mostly utilities. 
Every snippet will include an explanation of why you would use it and how it works.

-   [KeysOfUnion](keys-of-union.md)
-   [UnionToIntersection](union-to-intersection.md)
-   [NoInfer](no-infer.md)

## Notes & Articles

Here are a few common misconceptions, practices, and ideas that you should know.
Most of these are inspired directly by Stack Overflow questions (and answers).

-   [Conditional type abuse](conditional-type-abuse.md)
-   [`boolean` is a union](boolean-is-a-union.md)
-   [`keyof` with unions](keyof-with-unions.md)
-   [Generics with default values](generics-with-default-values.md)
-   [`as` is not a cast](as-is-not-a-cast.md)
-   [Mapped types:](mapped-types/README.md)
    - [Mapping over unions](mapped-types/mapping-over-unions.md)

## Issues & Pull Requests

This is a list of GitHub issues and pull requests I often reference, serving as a sort of landmark to build an answer upon.
I check for newer issues or pull requests to add every so often.

-   [#13298 Type manipulations: union to tuple](https://github.com/microsoft/TypeScript/issues/13298)
-   [#25784 Bring back typedef property expansion](https://github.com/microsoft/TypeScript/issues/25784)
-   [#33912 Relate control flow to conditional types in return types](https://github.com/microsoft/TypeScript/issues/33912)
-   [#41164 Circular reference error when defining Record type](https://github.com/microsoft/TypeScript/issues/41164)
-   [#44312 Keyword to permit inferring a union for a type parameter](https://github.com/microsoft/TypeScript/issues/44312)
-   [#47486 Empty object type is not working](https://github.com/microsoft/TypeScript/issues/47486)

## Quick links

This is just a list of webpages I link to extremely often.
Mostly just documentation, but sometimes there's a Stack Overflow Q&A or GitHub repository.

-   [Distributive conditional types](https://www.typescriptlang.org/docs/handbook/2/conditional-types.html#distributive-conditional-types)
-   [Type assertions](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-assertions)
