# EVM General Notes
max contract size of 24.49KB approx

can determine size with:
	`forge build --size`

## contract reuse/polymorphism

heavy reliance on the `constructor` :warn: in MUD where is this done, also MUD adds things into the worlds default name space and I dont know how.

### MUD/Lattice framework
* reuse seems to be largely a matter of `interfaces` and then passing addresses about and this deployment is handled in the `deployment` part of the framework which we haven't looked into. However there is a mechanism that may or may not be a Solidity idiomatic approach (in fact it seems to be): 