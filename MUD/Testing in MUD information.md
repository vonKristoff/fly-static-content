So, regarding tests: 

From "mud.dev": we need to import the general definitions that will be required for the tests:
-`MudTest` from lattice,
-the `World` and tables we will be using. 
The tests are Foundry tests "type" [https://book.getfoundry.sh/forge/tests](https://book.getfoundry.sh/forge/tests "https://book.getfoundry.sh/forge/tests") which means that any public function that starts with the word "test", will be a test executed.

Regarding the `World`, the MudTest gets the address from the `$WORLD_ADDRESS` environment variable and sets it as the Store address. Also, to get the size of the World contract we can use `extcodesize`. (edited)

To test using the  `World`, we need to:
This command runs the tests in a MUD project. Internally, it runs the following steps:

1. Starts an [`anvil`](https://book.getfoundry.sh/reference/anvil/) instance.
2. Deploys the `World` and all related `System`s using [`mud deploy`](https://mud.dev/cli/deploy).
3. Runs tests using [`forge test`(opens in a new tab)](https://book.getfoundry.sh/forge/tests) and passes the deployed world address to the tests via the `WORLD_ADDRESS` environment variable.


From "Foundry Book": to run the tests we use the command `forge test`. Usually tests will be placed in test/ and end with `.t.sol`. The default behavior is to only display a summary of pass or fail but, it is possible to control it by increasing the verbosity. If the test function reverts it means it fails, otherwise it passes. 
This chapter also mentions the different types of tests that can be done:
1. Fork mode: affords running an entire test suite against a specific forked environment. 
2. Fork Cheatcodes: provides more flexibility to work with multiple forks. 
3. Fuzz Testing: for property based testing. It also helps identify edge cases and unexpected behaviors in smart contracts by generating a wide range of inputs. 
4. Invariant testing: allows for a set of invariant expressions to be tested against randomized sequences of pre-defined function calls from pre-defined contracts. After each function call is performed, all defined invariants are asserted. (I think this more applied to wallet - contract - balance side)
5. Differential testing: can be used to compare the behavior of different implementations of a contract or to ensure that changes to a contract do not introduce bugs. For this one, we will need to have two versions of the contract we want con compare.

### Testing
1. Import the general definitions required in all MUD tests. MUD test contracts inherit from [MudTest](https://github.com/latticexyz/mud/blob/main/packages/world/test/MudTest.t.sol).
    1.1 // SPDX-License-Identifier: MIT
        pragma solidity >=0.8.21;
        import "forge-std/Test.sol";
        import { MudTest } from "@latticexyz/world/test/MudTest.t.sol";

2. Import the definitions required for this test, the World we can access and the tables we'll use.
    2.1 import { IWorld } from "../src/codegen/world/IWorld.sol";

3. MUD tests are [Foundry tests](https://book.getfoundry.sh/forge/tests). Any public function that starts with test is a test that gets executed.

4. The 'World' address comes from the MudTest. 'MudTest' gets the 'World' address from the '$WORLD_ADDRESS' environment variable and sets it as the 'Store' address.