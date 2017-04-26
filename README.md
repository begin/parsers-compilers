# parsers-compilers

> Lexers, tokenizers, parsers, compilers, renderers, stringifiers... What's the difference, and how do they work? 

- [lexer](#lexer)
- [parser](#parser)
- [compiler](#compiler)
- [renderer](#renderer)
- [related](#related)

## Lexer

> Sifts, or "tokenizes" the characters in a string to create an array of objects, referred to as a "token stream"

**Returns**: Token stream.

**Concepts**

- stream
- tokens
- token
- lexical scoping
- lexical context

**Example**

A JavaScript token stream for the string `abc{foo}xyz` might look something like this:

```js
[
  {
    type: 'text',
    value: 'abc',
    position: {start: {column: 1 line: 0}, end: {column: 3, line: 0}}
  },
  {
    type: 'left-brace',
    value: '{',
    position: {start: {column: 4 line: 0}, end: {column: 4, line: 0}}
  },
  {
    type: 'text',
    value: 'foo',
    position: {start: {column: 5 line: 0}, end: {column: 7, line: 0}}
  },
  {
    type: 'right-brace',
    value: '}',
    position: {start: {column: 8 line: 0}, end: {column: 8, line: 0}}
  },
  {
    type: 'text',
    value: 'xyz',
    position: {start: {column: 9 line: 0}, end: {column: 11, line: 0}}
  }
]
```

**Notes**

- "Tokens" are objects that contain details about the text "captured" for the token, such as column and row, or line number and character position.
- A token should also (and only) attempt to describe basic lexical context, such as character "type", which might be something like `text`, `number`, `escaped`, `delimiter`, or something similar.
- Lexers should not attempt to describe dynamic scope, this is left to the parser.  

## Parser

> Parses a stream of tokens into an AST

**Returns**: AST object

**Concepts**

- tree
- nodes
- node
- dynamic scoping
- dynamic context

**Example**

A JavaScript AST for the string `abc{foo}xyz` might look something like this:

```js
{
  type: 'root',
  nodes: [
    {
      type: 'text',
      value: 'abc',
      position: {start: {column: 1 line: 0}, end: {column: 3, line: 0}}
    },
    {
      type: 'brace',
      nodes: [
        {
          type: 'left-brace',
          value: '{',
          position: {start: {column: 4 line: 0}, end: {column: 4, line: 0}}
        },
        {
          type: 'text',
          value: 'foo',
          position: {start: {column: 5 line: 0}, end: {column: 7, line: 0}}
        },
        {
          type: 'right-brace',
          value: '}',
          position: {start: {column: 8 line: 0}, end: {column: 8, line: 0}}
        }
      ]
    },
    {
      type: 'text',
      value: 'xyz',
      position: {start: {column: 9 line: 0}, end: {column: 11, line: 0}}
    }
  ]
}
```

**Notes**

- Whereas token streams are "flat" arrays, a parser generates an _abstract syntax tree_ by giving the tokens in the stream a dynamic, or global, context

## Compiler

> Creates a function by converting an AST into a string of function statements and wrapping it with a boilerplate function body that defines the arguments the function can take. This generated function is then cached for re-use before being returned.

**Returns**: Function

**Concepts**

- function body
- function statements
- caching

**Notes**

The goal of a compiler is to create a cached function that can be invoked one or more times, on-demand, with the same or different arguments. The arguments passed to a compiled function are referred to as "context".

## Renderer

> Invokes the function returned from a compiler with a given "context", producing a string where any placeholders or variables that may have been defined are replaced with actual values.

**Returns**: String

**Concepts**

- context
- variables

## Related

- [the-super-tiny-compiler](https://github.com/thejameskyle/the-super-tiny-compiler)
- **stringify**: typically refers to converting an object to a string representation of the object. Example: `{foo: 'bar'}` would convert to the string `'{"foo": "bar"}'`.
- **assembler**: todo
- **interpreter**: todo
- **translater**: todo
