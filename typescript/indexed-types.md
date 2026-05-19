# Table names and indexed access types

TypeScript has some powerful type system features that I haven't seen in other languages. I recently learned about [indexed access types](https://www.typescriptlang.org/docs/handbook/2/indexed-access-types.html), which let me rewrite some code to make it easier _and_ safer to use correctly.

I have a UI prototype that makes SQL queries to the backend, then casts the response objects to the corresponding data type for the queried table. It looks something like this:

```typescript
await selectAll("repos") as Repo[];

async function selectAll(table: string): Promise<Row[]> {
  return select(`SELECT * FROM ${table}`);
}
```

where `repos` is the name of a table and `Repo` is a type we codegen from the schema representing a row (yes, the sql string building is Not Best Practice--it's a prototype!). Good APIs make it easy to get things right and this one falls short in two ways: you can get the table name wrong and you could make the wrong type assertion.

Fixing the table name problem is easy enough. We can codegen an enum or union type for the known tables. But that still leaves the problem of getting the cast right.

In other strongly typed languages, this is where I would flail around for a while and try to get something working. Typically it ends with some tradeoff between usability and safety where the author needs to specify more information. But in TypeScript it turns out to be easy to solve if we codegen a type like this:

```typescript
export type TableTypes = {
  repos: Repo,
  searches: Search,
  // ...
}
```

With just that type, you can rewrite `selectAll` like this:

```typescript
async function selectAll<T extends keyof TableTypes>(
  table: T,
): Promise<TableTypes[T][]> {
  const rows = await select(`SELECT * FROM ${table}`);
  return rows as TableTypes[T][];
}
```

The type parameter `T` uses `keyof` so the `table` parameter must be a key of the codegen `TableTypes`. This means that `selectAll("reops")` with a typo will fail to typecheck.

The second bit, which was new to me, is using the variable `T` to index into `TableTypes` for use in the return type annotation and type assertion. This is TypeScript's indexed access types, which let us express that the return is the type associated with that key.

With this we can write `const repos = await selectAll("repos")` and TypeScript automatically infers `Repo[]` without any additional assertions.
