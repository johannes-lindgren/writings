# The `never` type

Since a type represents a set of values, there exists a type that represents the _empty set_: this type is called `never`.

.The never type doesn't contain any values.
image:never.svg[]

In JavaScript, a value identifier always has a value, even if that value is `undefined`. Therefore, it is impossible to have an identifier with the type `never`. What use it is this type then? Later, we will learn that you can perform various operations on types, and sometimes, the `never` type shows up as a result of these operations. For now, it will not be important, but it's good to know that it exists to understand that types really are like sets.
