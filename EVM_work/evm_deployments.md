# EVM Deployments
## MUD
 needs to know about the chain config which seems to be hardcoded into the framework.

amend the following files:

`supportedChains.ts` and add a new chain
```ts
const fluentTestnet: MUDChain = {  
  name: "Fluent Testnet",  
  id: 1337,  
  network: "fluent-testnet",  
  nativeCurrency: { decimals: 18, name: "EtherDollar", symbol: "WZT" },  
  rpcUrls: {  
    default: {  
      http: ["https://rpc.dev1.fluentlabs.xyz"],  
      webSocket: [""],  
    },    public: {  
      http: ["https://rpc.dev1.fluentlabs.xyz"],  
      webSocket: [""],  
    },  },  blockExplorers: {  
    default: {  
      name: "Blockscout",  
      url: "https://blockscout.dev1.fluentlabs.xyz",  
    },  },  faucetUrl: "https://faucet.dev1.fluentlabs.xyz",  
} as const;
```

then export this chain config at the bottom

```ts
export const supportedChains: MUDChain[] = [mudFoundry, latticeTestnet, fluentTestnet];
```

then add the `chainid` as a query param 
`localhost:3000?chainid=1337`