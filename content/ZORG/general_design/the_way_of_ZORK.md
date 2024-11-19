## general ZORK engine design

ZORK is really a clever parser that leverages the fact that a sentence or command (in English at least) always starts with a verb and then contains a direct object or noun and a possible indirect object or noun and will also contain a bunch of other linguistic structures, prepositions, adverbs etc none of which in fact matter to the parser but they do matter to the experience of the user. 

The parser therefore finds verbs and objects and then matches them to a kind of phrase necessary to fulfil the game logic which is itself a fixed set of puzzles.
This being the case it also handles compound verbs such as "pick up" and "rusty sword", or "giant wasp bat" or what have you. It seems likely that it didn't try to be smarter than matching with some degree of fuzziness very particular phrases like "attack the duck with the rusty sword". It did also have some other tricks like counting the amount of times a meaningless phrase was entered amongst other to add to the aesthetics of the experience, however we are moving out of scope.

These parsed commands then generate a value that is then used in a lookup table to find the correct subroutine to run regarding that command, it is not of course a multiuser game so thats really all it needs, a bunch of commands, a bunch of game objects and a bunch of subroutines that have access to these game objects.

Presumably it also set a bunch of global state flags that then get referenced by the subroutines that hold the game "logic".

This seems to be the case for later InfoCom games but it's reasonable to assume this approach was also taken for the original ZORK adventure for which there is no generated source available, well at least that we are aware of.

That's essentially it, a set of mappings to subroutines. It's very tech appropriate. Its not too hard to imagine writing out a game script this way and then figuring out the subroutines and state vars needed.

I believe that this model evolved to a kind of IL that the ZORK/InfoCom tooling could read and spit out the subroutines and LUT's therefore allowing the games to be "scripted", at least that's what the code we have seen suggests and the articles we have read allude to and tbh I am too lazy to really research it.

There are plenty of other docs about this kind of thing and we aren't trying to be faithful (at all) to our ancestors but we can leverage this "tokenising" a phrase approach rather well to make a smart seeming parser that handles commands like "kick the football at the window", "kick ball", "go to the east", "go east", "east", etc. however we aren't interested in tokenising to match to a subroutine we want a more general system.

It's a cool model anyway.

### ZORK3000 or GTAEFk.n23 hereafter known as ZORG

There is a difference here though, we aren't looking for given phrases that then map to subroutines we are tokenising to extract "general" meaning.

We would like to have composability of some form and imagine this to mean users might add objects and rooms and puzzles etc to the world. So we want a more general form. 

We can't really add and don't think it's a great idea to allow the addition of new subroutines. New "things" by composing things yes but the world is the world, mystery meats are baked in at the start, we have fixed physics if you like.

To put it another way it isn't intended that user would add more "rules" i.e they couldn't add whole new mechanics like say fire if it didn't exist (which it happily does).

So then we come to the initial approach documented elsewhere [[meta_data_and_general_operations]] and we chain object graphs.

I guess what we are really doing is structuring the engine over its data using the Jackson Programming Model [wiki entry here](https://en.wikipedia.org/wiki/Jackson_structured_programming) because we know what our data will look like ergo we can loop through it quite happily. 

The engine tokenises a command and then hits a general action or verb handler with a few special cases, in our case LOOK and MOVE which handle things a little differently than the more general case but other than that we look for matching flags and this being the case we amend the game worlds state. Some states can be reset some cant.

The idea behind doing it this way is that (hopefully) we can expose the parser and handler API to allow for mods and also that we can write some tooling that allows for a kind of scripting of game worlds, the tooling would then, ZORK'ishly take this config as input and then generate the tables that contain the object graphs, the central contracts should "just" work because all actions get handled in a similar way.