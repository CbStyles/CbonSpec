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
  ```regex
    | [+-]? \d+[\d_]* ( '.' ( _*\d+[\d_]* )? )? ( [eE][-+]? _*\d+[\d_]* )?
    | [+-]? \.\d+[\d_]* ( [eE][-+]? _*\d+[\d_]* )?
  ```
  - hex  
    ```regex
    '0' 'x' _* [\da-fA-F]+ [\da-fA-F_]*
    ```
- word  
  ```regex
  [^\[\]\{\}\(\)'":=,;\s#]+
  ```
- date  
  - `<ISO 8601 Extended Date and Time>` [YYYY-MM-DDTHH:mm:ss.sssZ](https://tc39.github.io/ecma262/#sec-date-time-string-format)  
    ```regex
    <year> - ( 0[1-9] | 1[012] ) - ( 0[1-9] | [12]\d | 3[01] )
    T( [01]\d | 2[0-4] ) : [0-5]\d : [0-5]\d \. \d{3} ( Z | [+-]( [01]\d | 2[0-4] ) : [0-5]\d )
    ```  
  - Typed deserialization only `("'" <ISO 8601> "'") | ('"' <ISO 8601> '"")`  
  - year
    - .Net `\d{4}`
    - ECMAScript `( [+-]? \d{6} ) | \d{4}`
- uuid
  ```regex
  [\da-fA-F]{8} - [\da-fA-F]{4} - [\da-fA-F]{4} - [\da-fA-F]{4} - [\da-fA-F]{12}
  ```
- key  
  `string | word`
- array  
  `'[' (value | ',' | ';')* ']'`
- object  
  `'{' ( (key (':' | '=')? value) | ',' | ';' )* '}'`
- tagged union  
  `'(' key ')' value`
- value  
  `object | array | <tagged union> | string | word | date | uuid | number | bool | null`
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
