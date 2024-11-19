## node - pnpm for MUD
* install `pnpm`
	* install `nodenv` - `brew install nodenv` (needed for `node` management and `pnpm` needs `node`)
	* install node version for MUD (18.19.0) - `nodenv install 18.19.0`
	* set to local (to MUD) version to 18.19.0 - `nodenv local 18.19.0`
	* install `pnpm` - `brew install pnpm`
	* run `pnpm install` at project root

that should now allow the project to build, and to do this (somewhat OS dependent) is a matter of:

*  `pnpm dev` at the root of the project which should trigger a build of the client and the contracts and then run a local (`anvil`) deployment.
* this uses `mprocs` behind the scenes and this may not play nice on `x86_64` macs or at least it wasn`t for me. In that case use the `mprocs_dbg.yml` via the direct `mprocs` command `mprocs -c mprocs_dbg.yml`. That should do the same and also adds a bunch of extra debug stuff.

## RUST - Foundry etc
Needs rust installed and foundry:
see [Installing Foundry](https://book.getfoundry.sh/getting-started/installation)


## connecting `viem` to test nets from a local deploy

### MUD
* only understand's lattices test net.
* add a `MUDChain` type to `packages/client/src/mud/supportedChains.ts`
* add the deployed worlds address, this can be obtained either from calling the client with search params like `?chainId=123&worldAddress=0xae100...` or writing the `id` and `address` to the `packages/contracts/worlds.json` which in theory gets written to by a deployment.
* change the `transport:` field of the `clientConfig` instance to be of type `http()` you can add the full rpc url direct here or it falls back to the `chain.rpcUrls.default.http[0]` field. This is in `packages/client/src/mud/setupNetwork.ts` file
* add a query string to the request with the chainid `http://localhost:300?chainid=1337` then this gets passed into the setup functions correctly

### using `pnpm`
	
there is a `package.json` defined action that handles the build and run of view that works assuming the above stuff is setup. see `packages/client/package.json`

`pnpm dev-fluent`


### CORS and faucet
`setupNetwork.ts` checks the connected wallet for funds and should they be low then uses the faucet for drip feed, this just errors out and presumably is only viable on redstone ? 
se we use an env var

### server component locally
provided by [Vite](https://vitejs.dev/guide/build.html)
set private key for burner in the `pckages/client/.env` for now
any vars prepended with `VITE_` will be picked up with `import.meta.env.VITE_VAR_NAMER`
also don't forget to add the search query for chain id, this should be 1337 i.e `?chainid=1337` this ensures that the correct params get generated see: `getNetworkConfig.ts`

### `metamask` and wallet client	

`pnpm install @metamask/sdk` in `packages/client`: `getNetworkConfig.ts` :warn: you need to install the `metamask` browser extension to get the dev tools console  to open because otherwise the JS will error out pre loading this in... 
