# vdf-parser

`vdf-parser` is a library that can convert VDF to JSON and vice versa.

It is mostly based on [rossengeorgiev/vdf-parser](https://github.com/rossengeorgiev/vdf-parser) and includes some features inspired by [RoyalBingBong/vdfplus](https://github.com/RoyalBingBong/vdfplus) (which lacked some features supported by the former...), but contains many new features.

Format: https://developer.valvesoftware.com/wiki/KeyValues

VDF may contain comments. However, they are not preserved during decoding.

## Features

- Supports unquoted keys and values
- Supports keys that appear multiple times by "arrayifying" them – whether they contain child objects or other values
- Supports uppercase characters in keys and values (and unquoted floats in values)
- Supports value type recognition and automatic conversion (booleans, integers, floats)
- Supports non well formed objects (missing newlines), for example: `"1" { "label" "#SFUI_WinMatchColon" "value" "#SFUI_Rounds" }`
- Recognizes conditionals (ex. `[$WIN32||$X360]`), but skips their interpretation
- Includes TypeScript types

## Methods

```ts
/**
 * Parse a VDF string into a JavaScript object
 * @param text VDF text
 * @param options Parsing options. Accepts a boolean for backwards compatibility ("types" option defaulting to true)
 * @returns Parsed object
 */
export function parse(text: string, options?: VDFParseOptions | boolean): object;

/**
 * Parse a JavaScript object into a VDF string
 * @param obj The object to stringify
 * @param options Parsing options. Accepts a boolean for backwards compatibility ("pretty" option defaulting to false)
 * @returns VDF string
 */
export function stringify(obj: object, options?: VDFStringifyOptions | boolean): string;
```

where options are as follows:

```ts
interface VDFParseOptions {
    /**
     * Attempt to automatically convert numbers and booleans to their correct types, defaults to true
     */
    types: boolean = true;

    /**
     * Arrayify the values if they appear multiple times.
     * Enabled by default, because Source does support multiple values with the same key (as separate entries).
     * One may want to disable it if they expect a single value and their code is not prepared for different cases.
     * In such case, the existing text value would be replaced with the new one, and existing object patched with the new values.
     */
    arrayify: boolean = true;
}

interface VDFStringifyOptions {
    /**
     * Add indentation to the resulting text, defaults to false
     */
    pretty: boolean = false;

    /**
     * Indent with the following characters, defaults to a tabulator, requires "pretty" to be set to true
     */
    indent: string = "\t";
}
```

## Installation

`npm install vdf-parser`

## Usage

```js
const VDF = require('vdf-parser');
var parsed = VDF.parse(text);
```

or

```js
import * as VDF from 'vdf-parser';
let parsed = VDF.parse(text);
```
