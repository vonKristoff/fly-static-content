# Players and Authors Goals

In current Oruggin Trails, there is no much of any kind of goals for Players or Authors.

We could argue that authors have a goal of creating meaningful stories for players to play and players have for goals to solve the riddle of the rooms and their intricate mechanism designed by the authors.

But Oruggin Trail is running on a decentralised network which comes with its own cost and intricacies that do not really match these goals.

In particular, an onchain game systems are fully transparent and so any intricate mechanism implemented without carefull consideration of such transparency would be trivial to solve without the need to actually play the game.

It is also worth pointing out that even with careful use of technology like zk or commit+reveal, any author secret would be at the mercy of the author discretion. And such author would thus carry the responsability of the potential value assoicated with puzzle solving.

## Why putting such game onchain ?

Since putting a game onchain has its own intricacies (cost, transparency), it is worth asking ourselves: what would Oruggin Traill gains in being there ?

1. We leverage the open api that smart contract provide ?

2. we leverage the possibility of adding incentives, both external (from other projects) and internal (token design) ?

3. We leverage the enthusiasm of the onchain community to play with it and build on it ?

These are possible benefits, let's look at them in details

### 1. We leverage the open api that smart contract provide ?

Or in other words: by being fully transparent, Oruggin Trail offer a unique experience to player interested in building atop.

This is indeed a benefit in that the network the game would run on also provide an open api and a mechanism to script anything on top, without any need for us to build it. Composability is given, at least in its raw form.

The EVM / Cairo-VM also comes with the benefit of gas metering which allow extension to compute without affecting the game.

The issue though is that these network have a cost. The cost is there for different reason but one of them is DDOS protection. Removing that cost and your transparent system is prey to attack.

If there is cost for building rooms and playing them, what are players incentives to actually play there, especially since the game is fully transparent, they could play it offchain for free.

In other words, what are the benefit for the players and authors compared to forking it and have it run on traditional server infra ?

### 2. we leverage the possibility of adding incentives, both external (from other projects) and internal (token design) ?

Incentives in traditional ecosystem is much harder to implement and having the game onchain will indeed make it much easier to allow external agent to incentivise the creation of rooms.

Similarly, a token could be designed to both incentivize and reward the community building on the Oruggin Trail.

But then, when incentive are in place, the issue related to solving puzzle with automatism become far more important.

### 3. We leverage the enthusiasm of the onchain community to play with it and build on it ?

This can indeed be a way to bootstrap a community but the problem mentioned above still stand for the long term.

## What then ?

### PVP

An easy way to add goal is to set players against each other and have them potentially lose or win something.

Oruggin Trail's theme could be leveraged to set players in camp.

Ignoring for now the authors, players could be in one camp having a specific goal

For example one camp's goal could be: "spread a disease". The other camp would then have for goal to survive and eliminate the disease.

To ensure fairness, every time a player chose one camp, the other camp cost of entry get reduced.

The cost of entry could then be added to a jackpot that is then used to reward winners.


### What about authors ?

While PVP with players is relatievely straighforward, the case of authors is more complex.

How can then win ? How can then lose ?

One idea is that they are the one setting up the world to be played in and so they have incentives to make the system fair for both camp if they hope player to join in.

They could then be rewarded by taking a cut of the jackpot.
