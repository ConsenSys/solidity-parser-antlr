[<img width="200" alt="get in touch with Consensys Diligence" src="https://user-images.githubusercontent.com/2865694/56826101-91dcf380-685b-11e9-937c-af49c2510aa0.png">](https://diligence.consensys.net)<br/>
<sup>
[[  🌐  ](https://diligence.consensys.net/?utm_source=github_npm&utm_medium=banner&utm_campaign=solidity-parser-diligence)  [  📩  ](mailto:diligence@consensys.net)  [  🔥  ](https://consensys.github.io/diligence/)]
</sup><br/><br/>

[![npm version](https://badge.fury.io/js/solidity-parser-diligence.svg)](https://badge.fury.io/js/solidity-parser-diligence)

solidity-parser-diligence
=====================

A Solidity parser built on top of [ANTLR4 (ANother Tool for Language Recognition)](https://www.antlr.org/). The ANTLR4 grammar file for Solidity [Solidity.g4](https://github.com/ConsenSys/solidity-antlr4/blob/master/Solidity.g4) is maintained in the [solidity-antlr4](https://github.com/consensys/solidity-antlr4) repository.

Now maintained by the ConsenSys Diligence team! :tada:

You can find this new package in NPM at [solidity-parser-diligence](https://www.npmjs.com/package/solidity-parser-diligence).

## Install

The following installation options assume [Node.js](https://nodejs.org/en/download/) has already been installed.

Using [Node Package Manager (npm)](https://www.npmjs.com/).

```
npm install solidity-parser-diligence
```

Using [yarn](https://yarnpkg.com/)

```
yarn add solidity-parser-diligence
```

## Usage

```javascript
import parser from 'solidity-parser-diligence';

var input = `
    contract test {
        uint256 a;
        function f() {}
    }
`
try {
    parser.parse(input)
} catch (e) {
    if (e instanceof parser.ParserError) {
        console.log(e.errors)
    }
}
```

The `parse` method also accepts a second argument which lets you specify the
following options, in a style similar to the _esprima_ API:

| Key      | Type    | Default | Description                                                                                                                                                                                          |
|----------|---------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| tolerant | Boolean | false   | When set to `true` it will collect syntax errors and place them in a list under the key `errors` inside the root node of the returned AST. Otherwise, it will raise a `parser.ParserError`.          |
| loc      | Boolean | false   | When set to `true`, it will add location information to each node, with start and stop keys that contain the corresponding line and column numbers. Column numbers start from 0, lines start from 1. |
| range    | Boolean | false   | When set to `true`, it will add range information to each node, which consists of a two-element array with start and stop character indexes in the input.                                            |


### Example with location information

```javascript
parser.parse('contract test { uint a; }', { loc: true })

// { type: 'SourceUnit',
//   children:
//    [ { type: 'ContractDefinition',
//        name: 'test',
//        baseContracts: [],
//        subNodes: [Array],
//        kind: 'contract',
//        loc: [Object] } ],
//   loc: { start: { line: 1, column: 0 }, end: { line: 1, column: 24 } } }

```

### Example using a visitor to walk over the AST

```javascript
var ast = parser.parse('contract test { uint a; }')

// output the path of each import found
parser.visit(ast, {
  ImportDirective: function(node) {
    console.log(node.path)
  }
})
```

## Contribution

This project is dependant on the [solidity-antlr4](https://github.com/consensys/solidity-antlr4) repository via a git submodule. To clone this repository and the submodule, run

```
git clone --recursive
```

If you have already cloned this repo, you can load the submodule with 

```
git submodule update --init
```

This project can be linked to a forked `solidity-antlr4` project by editing the url in the [.gitmodules](.gitmodules) file to point to the forked repo and running

```
git submodule sync
```

The Solidity ANTLR file [Solidity.g4](./solidity-antlr4/Solidity.g4) can be built with the following. This will download the antlr jar file to [solidity-antlr4/antlr4.jar](./solidity-antlr4/antlr4.jar) if it doesn't already exist.

```
yarn run build
```

The [mocha](https://mochajs.org/) tests under the [test](./test) folder can be run with the following. This includes parsing the [test.sol](./test/test.sol) Solidity file.

```
yarn run test
```

## Authors

Gonçalo Sá ([@gnsps](https://twitter.com/gnsps))

Federico Bond ([@federicobond](https://github.com/federicobond))

## License

MIT
