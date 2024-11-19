# Starknet Contracts

#dojo-contracts

It's worth going through the basecamp videos and in particular the session about smart contracts.

The actual workshop is on github here [counter workshop](https://github.com/starknet-edu/counter-workshop)


## dojo oddities 
#dojo #errors

lots of errors get hidden by the compiler and end up being stuff like `foo blah garble #[dojo::contract]` i.e something is fucking up the macro or rather the macro expands and something else is fubar and you dont get an erro

`dojo_init` the constructor, we can use this to pass in stuff and use overlays to get this stuff into it also can call it from test functions and use the `callData` shizzle.

If we do have this function in the code base and we then init the contract and try to call an `abi` that we have exposed in the `interface` then it seem to blow things up. I guess because of the `dojo_init` looking for params that aren't present in our calling code?? either way just comment this out and then the data is passed into the interface just fine

## model paths in test

we need to use the `use the_oruggin_trail::models::mymodel::{Mymodel, mymodel};` syntax and there seems to be some kind of wierd codegen smell here.

scope path rules seem borked

`use the_oruggin_trail::models::mymodel::{MyModel, mymodel};` wont work perhaps there is a one to one match somewhere?

also fname and name must match other than capitalisation

regardless just use models/mymodel and make the struct Mymodel then you can use the `mymodel import` and the `models = array![mymodel::TEST_CLASS_HASH]` to deploy them in tests, otherwise it just fails horribly

