# @thi.ng/rstream-dot

[![npm (scoped)](https://img.shields.io/npm/v/@thi.ng/rstream-dot.svg)](https://www.npmjs.com/package/@thi.ng/rstream-dot)

This project is part of the
[@thi.ng/umbrella](https://github.com/thi-ng/umbrella/) monorepo.

## About

Graphviz DOT conversion of @thi.ng/rstream dataflow graph topologies.

## Installation

```
yarn add @thi.ng/rstream-dot
```

## Usage examples

```typescript
import * as rsd from "@thi.ng/rstream-dot";

import * as rs from "@thi.ng/rstream";
import * as tx from "@thi.ng/transducers";

a = rs.fromIterable([1,2,3]);
b = rs.fromIterable([10, 20, 30]);

a.transform(tx.map((x)=> x*10), "x10");

rs.merge({src: [a,b]}).subscribe(rs.trace());

// now capture the topology by walking the graph from its root(s)
console.log(rsd.toDot(rsd.walk([a,b])));

// digraph g {
// rankdir=LR;
// node[fontname=Inconsolata,fontsize=11,style=filled,fontcolor=white];
// edge[fontname=Inconsolata,fontsize=11];
// s0[label="iterable-0\n(Stream)", color=blue];
// s1[label="x10", color=black];
// s2[label="in-iterable-0", color=black];
// s3[label="<noid>", color=gray];
// s4[label="streammerge-0\n(StreamMerge)", color=red];
// s5[label="sub-1", color=black];
// s6[label="<noid>", color=gray];
// s7[label="iterable-1\n(Stream)", color=blue];
// s8[label="in-iterable-1", color=black];
// s9[label="<noid>", color=gray];
// s5 -> s6;
// s4 -> s5;
// s3 -> s4;
// s2 -> s3;
// s0 -> s1[label="xform"];
// s0 -> s2;
// s9 -> s4;
// s8 -> s9;
// s7 -> s8;
// }
```

Copy output to file `graph.dot` and then run:

```
dot -Tsvg -o graph.svg graph.dot
```

This will generate this diagram:

![graphviz output](../../assets/rs-dot-example.svg)

## Authors

- Karsten Schmidt

## License

&copy; 2018 Karsten Schmidt // Apache Software License 2.0