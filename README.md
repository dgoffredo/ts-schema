TypeScript Schema Checker
=========================
Validate JSON values against a TypeScript interface.

If you google this idea, there's a [Hacker News thread][1].

Usage
-----
```console
$ cat example/schema.ts
interface Person {
	firstName: string,
	lastName: string,
	age?: number
}

$ cat example/input.json
{
    "firstName": "Sonny",
    "lastName": "Rollins",
    "age": "this should be a number"
}

$ ./ts-schema-check example/schema.ts example/input.json 
../../../../../tmp/3RdBdG/source.ts:4:5 - error TS2322: Type 'string' is not assignable to type 'number'.

4     "age": "this should be a number"
      ~~~~~

  ../../../../../tmp/3RdBdG/source.ts:11:2
    11  age?: number
        ~~~
    The expected type comes from property 'age' which is declared here on type 'Person'


Found 1 error in ../../../../../tmp/3RdBdG/source.ts:4

$ echo $?
1
```

If the JSON _does_ match the schema, then the script returns exit status zero
and doesn't produce any output.

Dependencies
------------
- `/dev/stdin`
- `/usr/bin/env`
- `tsc`, the TypeScript compiler, must be visible to `/usr/bin/env`.
- `/tmp/`

More
----
Consult `--help`, or read [the source](ts-schema-check).
```console
$ ./ts-schema-check --help
ts-schema-check — Validate JSON against a TypeScript interface.

Usage:

    ts-schema-check <SCHEMA_FILE> [<JSON_FILE>]
        Validate the JSON value in the optionally specified JSON_FILE
        against the TypeScript interface in the specified SCHEMA_FILE.
        If the JSON satisfies the interface, return status code 0.
        If the JSON does not satisfy the interface, print a diagnostic to
        standard error and return status code 1.
        If an error occurs, return status code 2.
        If JSON_FILE is not specified or is "-", then read the JSON value from
        standard input.

    ts-schema-check --help
    ts-schema-check -h
        Print this message.
```

[1]: https://news.ycombinator.com/item?id=16407243
