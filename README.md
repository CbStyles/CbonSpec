# CbonSpec

- Version 1.0.0
- Full name is `Common Bracket Object Notation`
- No comment  
- Allow multiple documents per file  
- The document root is `value`

## Syntax
- null  
  `'null'`
- bool  
  `'true' | 'false'`
- string  
  `("'" (<escape>|<anychar>) "'") | ('"' (<escape>|<anychar>) '"')`
- number  
  ```regexp
    ( \d+[\d_]*  (\.(\d+[\d_]*)?)?  ([eE][-+]?\d+[\d_]*)?   )
  | ( \.\d+[\d_]*  ([eE][-+]?\d+[\d_]*)?                    )
  ```
  - hex  
    ```regexp
    0x[\da-fA-F]+[\da-fA-F_]*
    ```
- word  
  ```regexp
  [^\[\]\{\}\(\)'":=,;\s#]+
  ```
- date  
  - `<ISO 8601>`
  - Typed deserialization only `("'" <ISO 8601> "'") | ('"' <ISO 8601> '"")`
- key  
  `string | word | number | bool | null | date`
- array  
  `'[' (value | ',' | ';')* ']'`
- object  
  `'{' ( (key (':' | '=')? value) | ',' | ';' )* '}'`
- tagged union  
  `'(' key ')' value`
- value  
  `object | array | <tagged union> | string | word | date | number | bool | null`
- escape  
  `'\' <the escape>`
  - `'\'` -> `'\'`
  - `"'"` -> `"'"`
  - `'"'` -> `'"'`
  - `'a'` -> `'\u{7}'`
  - `'b'` -> `'\u{8}'`
  - `'t'` -> `'\u{9}'`
  - `'n'` -> `'\u{A}'`
  - `'v'` -> `'\u{B}'`
  - `'f'` -> `'\u{C}'`
  - `'r'` -> `'\u{D}'`
  - `'u' '{' [\da-fA-F]{1,6} '}'` -> `<unicode char>`
  - `'u' [\da-fA-F]{1,6}` -> `<unicode char>`
  - `<other char>` -> `<other char>`
