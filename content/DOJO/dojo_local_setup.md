# setting up locally using `sozo` #sozo #dojo #setup

you need the tooling for dojo see [[dojo_tools_setup]] and the [dojo docs getting started](https://book.dojoengine.org/getting-started)

now you can use `sozo` to scaffold out a project.
	```sozo init my_shiny_thing```

that clones out the default starter project [dojo-starter-repo](https://github.com/dojoengine/dojo-starter/tree/main)

_BUT_ the tool will not itself update the projects `Scarb.toml` nor the dependencies in the stubbed out models etc.

you can however follow through the steps çand it will all run.

if you want to amend the `Scarb.toml` to reflect whatever you passed to ```sozo init my_shiny_thing``` then you need to fix up the naming in the checked out files

use `sed` for this, run it from the root of the project.
you may need to fix the command for your OS and that probably means dropping the `''` part which is for BSD's (like Mac).

    find src -type f -exec sed -i '' 's/dojo_starter/foobar/g' {} +

now you want to clean up under `manifests`. just rm everything under that dir.

to regenerate the abi's etc you need `sozo` again

    sozo build

## running locally without a script #deploy
this is copied out of the link above [dojo-starter-repo](https://github.com/dojoengine/dojo-starter/tree/main)

`sozo build`

`katana --disable-fee`

`sozo migrate apply` - then copy the World's contract address from the output

copy the address into the `Scarb.toml [tool.dojo.env]::world_address` field

`tori --world <world_address>`

you should now have a local node running and `torii` indexing the deployed contract and providing a GraphQL API at `localhost:8080/graphql`

## build script for a local torii and katana deploy

#### deploy #deploy #pnpm #tooling

there is a script `scripts/dev_deploy` that can be run to automate this
it will:
* run `sozo build`
* deploy an instance of katana
* run `sozo migrate apply` to deploy the world
* then get the worlds address and run the `torii` indexer

* build_manifest. this is used by `pnpm` from the vue project root. i.e. its added as a `"scripts"` key in the root `package.json`

to use it call `./scripts/dev_deploy.sh run` or just use `pnpm` from the project root, this will call all the intermediate steps.

`pnpm deploy:local`

#### cleanup
to cleanup (i.e. stop `torii` and `katana`) run `./scripts/dev_deploy.sh cleanup`

#### monitoring

the output from the script will give you 2 `l_path` vars one for `torii` and one for `katana` because we background the running processes.

so if you want to monitor the output you will want to check the log files, I just use `tail -f <log_path>` and the easiest way is to dump this into `mprocs`

``` shell
mprocs "tail -f <katana_log_path>" "tail -f <torii_log_path>"
```

### deps
`tom-cli` rust tool to read in .toml files and output as json

### network calls RPC gotchas

**NOTE** dont use `localhost:5050` because `katana` doesn't listen on that interface so the calls will immediately and we will get a `500`
