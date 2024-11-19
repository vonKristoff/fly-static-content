# Setting up Cairo and Dojo Toolchains


## toolchain

we can install the `cairo` tool chain or `dojo`.

### cairo dev
For a complete list of instructions see the top of the README.md from the BaseCamp repo available here [counter workshop](https://github.com/starknet-edu/counter-workshop?tab=readme-ov-file)

what we do is as follows:

install dependencies:

* `asdf` - package manager uses shims like pyenv/rbenv/nodenv etc, [asdf](https://asdf-vm.com/guide/getting-started.html) 	
	
* `scarb` via `asdf` -  compiler [scarb](https://docs.swmansion.com/scarb)
	*  ```asdf plugin add scarb```

* `snforge` - starknet foundry via asdf - [starknet foundry](https://foundry-rs.github.io/starknet-foundry/getting-started/first-steps.html)
	* ```asdf plugin add starknet-foundry```
	* ```asdf install starknet-foundry latest```

* Cairo language server, in VSCode via the VSCode marketplace.


for a nice video talking through this see around 7:00 in this workshop [starknet-basecamp](https://www.youtube.com/watch?v=RFV_COjDmPI)

### dojo

itself a set of cairo tooling that bundles a framework and indexer and sequencer  and a build/migration tool see [dojo tool](https://foundry-rs.github.io/starknet-foundry/getting-started/first-steps.html)

* `dojo` via `asdf` plugin. 
	* ```asdf plugin add dojo https://github.com/dojoengine/asdf-dojo```
	that will bung shims in your path etc
	

* OR use `dojoup` itself a dojo package manager. 
	* ```curl -L https://install.dojoengine.org | bash``` - to install `dojoup`
	* `dojoup`  to install latest `dojo`
* I would __strongly__ advise to NOT use `dojoup` but use `asdf`

### other IDE's
* intellij setup (as of 26/5 haven't got intellij all hooked up and tbh its a dead loss I think...)
	there is a plugin https://github.com/kahondev/cairo-jetbrains that uses this LSP https://github.com/starkware-libs/cairo/blob/main/vscode-cairo/README.md#install-the-cairo-language-server

* hello_world
	* see this guide [dojo-starter](https://book.dojoengine.org/tutorial/dojo-starter) for a quick walk through and to get something running locally

### VSCode
* `vscode` should just work with the LSP assuming it's installed which it should be via `scarb` but the link https://github.com/starkware-libs/cairo/blob/main/vscode-cairo/README.md#install-the-cairo-language-server has more info. You also need to install the Cairo1 extension.

* LanguageServer : a fresh install seems to *just work* for me at least and so long as there is a `Scarb.toml` at the project root it seems happy.
* we _probably_ want to use the actual binary which should be at something like $HOME/.asdf/installs/scarb/<version>/bin/scarb-cairo-language-server
* ~~just use the shims from `.asdf` and make use you have a functional `Scarb.toml` at the root~~
* this is supposed to just be started by `VSCode` 
* [language server home page](https://marketplace.visualstudio.com/items?itemName=starkware.cairo1)
