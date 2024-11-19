# Getting Started
## Introduction

This is intended as a simple(ish) design doc of the ZORK3000, or the GTAEFk.n23, or just the "engine". It has links out to more detailed descriptions and covers the following:

* a general look at the inspiration for the design, [[the_way_of_ZORK]] , gives an overview of the ZORK system in so far as we have taken inspiration. 

* a look at the design of the engine from the point of view of it's data and operations, [operations/meta_data_and_general_operations]], this covers at a high level at least how we chain data to have puzzles and how we handle parsing strings into tokens and thus actions or commands in the game.

* a look into how we derive a syntax for parsing commands [[actions_VERBS]] examines some simple `EBNF` type grammar's for verbs and then discusses the design of the algo's that handle these.

* a look into how we handle `composites` e.g. "pick up the ..." [[Composite forms]] has been started here #todo 

* how to add a room, [[what_is_a_room]] here we go into more depth on how to set up a room and its objects and then as such how the engine actually does stuff. 

* missing #todo the tables design (i.e. the metadata), some more logic, how to parse a config into a world, the systems involved etc. 


## installation for dev

### MUD
#### dependencies
* `foundry` rust tooling that handles compilation and deployment. [foundry](https://book.getfoundry.sh/getting-started/installation) 
* `pnpm` [pnpm](https://pnpm.io/installation)
* `node` @ v 18.19.x via `nvm`, or `nodenv` [nvm](https://github.com/nvm-sh/nvm?tab=readme-ov-file#installing-and-updating), [nodenv](https://github.com/nodenv/nodenv?tab=readme-ov-file#installation)

### the o'ruggin train, geta n23, tot etc

* clone the repo; cd into its root, set the node version, run `pnpm install`
* if that all works run `pnpm dev`.
* if you are on an older `x86_64` mac you will need to install `mprocs` yourself via `brew` or what have you and use `mprocs` directly, i.e. `mprocs -c mprocs_dbg.yaml`