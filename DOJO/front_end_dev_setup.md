# Front End Development Setup #setup #frontend #dojo 


## using dojo.js & GraphQL #graphql

indexer is torii
#dojo-js #graphql #subscriptions

* can examine the sqlite database by mounting a file path at startup then can query models etc a bit easeir as see what torii knows
* torii and dojo consider things as `entities` and `componenets`, `components` are the models. `entities` are loose abstraction and are a collection of models.
* The composition is achieved by collecting the VALUES of the model `#[keys]` into a `poseidon` hash with the VALUES of keys of the models.
 
 once we have a hash that becomes the `id` of the entity so we can create a GraphQL subscription on the entity as below:
 ```json
 subscription {

  entityUpdated(id: "0x2038e0daba5c3948a6289e91e2a68dfc28e734a281c753933b8bd331e6d3dae") {

     keys,

     models {

    ... on Output {

      text_o_vision

    }

    }

  }

}
```

#dojo-js 
`dojo.js` has a couple of functions for this: see [poseidon hash](https://github.com/dojoengine/dojo.js/blob/58751dcc52d22af9eb98afaa66a22b00744616b5/packages/utils/src/utils/index.ts#L181C17-L181C35)

the actual repo `micro-starknet` with the hash fucntion is [micro-starknet](https://github.com/paulmillr/micro-starknet)

the actual function [poseidinHasMany](https://github.com/UNazariiSA/micro-starknet/blob/main/index.ts)

also another set of contract calls but not `torii` calls in `dojo.js` `packages/core/provider/DojoProvider.ts`

## jsonRPC Calls
when local dev this uses `katana`
### katana
default keys account (seed0) for burner deployed by katana at spinup this is used by the `sozo migrate apply` call that actually deploys the contracts

```toml
[tool.dojo.env]
rpc_url = "http://localhost:5050/"
# Default account for katana with seed = 0
account_address = "0xb3ff441a68610b30fd5e2abbf3a1548eb6ba6f3559f2862bf2dc757e5828ca"
private_key = "0x2bbf4f9fd0bbb2e60b0316c1fe0b76cf7a4d0198bd493ced9b8df2a3a24d68a"
world_address = "0x5112adeb35112eccb3f3bc823bac1bfe73693a0872f7a579045e510e9219b49"
```

the full Katana output for keys/accounts [0..2] etc is as follows:

```
PREFUNDED ACCOUNTS
==================

| Account address |  0xb3ff441a68610b30fd5e2abbf3a1548eb6ba6f3559f2862bf2dc757e5828ca
| Private key     |  0x2bbf4f9fd0bbb2e60b0316c1fe0b76cf7a4d0198bd493ced9b8df2a3a24d68a
| Public key      |  0x640466ebd2ce505209d3e5c4494b4276ed8f1cde764d757eb48831961f7cdea

| Account address |  0xe29882a1fcba1e7e10cad46212257fea5c752a4f9b1b1ec683c503a2cf5c8a
| Private key     |  0x14d6672dcb4b77ca36a887e9a11cd9d637d5012468175829e9c6e770c61642
| Public key      |  0x16e375df37a7653038bd9eccd767e780c2c4d4c66b4c85f455236a3fd75673a

| Account address |  0x29873c310fbefde666dc32a1554fea6bb45eecc84f680f8a2b0a8fbb8cb89af
| Private key     |  0xc5b2fcab997346f3ea1c00b002ecf6f382c5f9c9659a3894eb783c5320f912
| Public key      |  0x33246ce85ebdc292e6a5c5b4dd51fab2757be34b8ffda847ca6925edf31cb67

```

example code here: [phillipeR26 tuturials - contract call](https://github.com/PhilippeR26/starknet.js-workshop-typescript/blob/main/src/scripts/11.CallInvokeContract.ts)

and starknet [general docs on rpc setup](https://www.starknetjs.com/docs/guides/intro)

NOTE: we need to use the `invoke` pattern on the rpc calls as `call` is for read only functions. ALso we need to pass a `ref` not a `@` in the function params if we wish to actually `write` to the world state

### outputter toml
```
[[contracts]]
kind = "DojoContract"
address = "0x5351273085d5dfbf7ab213b6712cd0cd81b12eefcfa278b8f2791e9061af146"
class_hash = "0x17d39ed4aa24ad055c75bebf11c0c1d3abe36ecf0ede717975d3a09cab06fdf"
original_class_hash = "0x17d39ed4aa24ad055c75bebf11c0c1d3abe36ecf0ede717975d3a09cab06fdf"
base_class_hash = "0x22f3e55b61d86c2ac5239fa3b3b8761f26b9a5c0b5f61ddbd5d756ced498b46"
abi = "manifests/dev/abis/deployments/contracts/the_oruggin_trail_systems_outputter_outputter.json"
reads = []
writes = []
computed = []
init_calldata = []
name = "the_oruggin_trail::systems::outputter::outputter"
```



### sqlite
the torii indexer used sqlite under the hood we can pass in a path to a db file so we can check this in dev:
`--database /tmp/torii`


### troubleshooting #fixes

#### tool chain
* check your versions, `Scarb.toml` :
				
				`cairo-version = "2.6.3"`
				`dojo = { git = "https://github.com/dojoengine/dojo", tag 
				= "v0.7.3" }`
		
	 make sure they match output from `sozo --version`, you want something like
			
			> sozo --version
			sozo 0.7.3
			scarb: 2.6.3
			cairo: 2.6.3
			sierra: 1.5.0

	and `which sozo` should output something like:
	```
	> which sozo
	/Users/tims/.asdf/shims/sozo
	```

#### vscode cairo language server

check the output in VSCode but sometimes it doesn't like shims in the LS path
	
`asdf which scarb` 
	
will output the path that `asdf` is using for the tool.

```
> asdf which scarb
/Users/tims/.asdf/installs/scarb/2.6.3/bin/scarb

```
			



## NOTES
codegen:? /Users/tims/DATA/BB/dojo/tot_vue/dojo.js/packages/core/bin/generateComponents.cjs