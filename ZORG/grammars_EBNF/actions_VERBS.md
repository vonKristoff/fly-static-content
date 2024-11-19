## GENERAL
eg: 
* "kick the ball at the window"
* "break the bottle"
* "smash window"
* "smash window with rock"
* "unlock door"
* "unlock the door with key"
* "burn hay with matches"
* "light hay with matches"
* "light candle"
* "light candle with matches"
* "light the candle with the matches"
* "throw the bottle at the cop"

there are several types of verbs we could use in the game:
1. `transitive`:
	* these require a direct object, eg. `kick` requires an object such as `ball` it can also contain a preposition `at` and an indirect object `window`, we treat `go` as a special case for the games purposes.
	These is the main type we need to parse for.
2. `phrasal` or `compound`: #todo 
	* these require a `preposition` and a direct object, eg: `pick up ball`
	We essentially ignore these right now in favour of `transitive` such as `take`. We also treat `look` as a special case.
3. `intransitive`:
	* these do not require a direct object, eg: `look`, `run`, `jump`
	We treat these as short forms or aliases. They can of course also be made transitive by adding the preposition.
		
So then the list above are all examples of `transitive` verbs and we can make a grammar as below:

```ebnf
(* transitive verbs*)
t_cmd = vrb, [definiteArticle], obj, [( prePosition, [definiteArticle], obj )];
definiteArticle = "the";
prepPosition = at | with | on | up | ...;
at = "at";
with = "with";
...;
obj = noun;
vrb = hit | throw | burn ...;
hit = "hit";
...;
```

this grammar does not include composite verbs as mentioned above and needs to be added the amended grammar would add the prepPosition to the string lit to map to the given compound verbs as below #todo 
```ebnf
vrb = take | hit | ...;
take = ("pick", prepPosition) | "pick";
```

ignoring the compound form mention above the unamended logic can use the final `obj` to shortcut the logic.

This ends up being surprisingly simple:
![Local Image](file:///Users/tims/desktop/trans_verb.jpg)

Some commands can be special cases for the logic of the game, right now this is just __LOOK__.

It's also possible for the purpose of the game to use __ALIASES__ or shortened forms essentially. 

## LOOK (phrasal, intransitive, transitive)
```ebnf
l_cmd = la | lb | lar | lf | li | d_cmd; 
la = look, (at | to), [the], obj | place | dir;
lb = look, behind, obj;
lar = look, around, [place];
lf = look, for, obj, [(in, place | obj)];
li = look, inside, obj;
lk = look;

d_cmd = desc, [the], obj | place;

obj = place | thing;
place = room | cabin | cave | plain | forest...; 
thing = door | football...; 
dir = n | e | s | w | u | d;
room = "room";
cabin = "cabin";
look = "look";
...
(* you get the idea *)
```

We treat  `look` as both `transitive` and `intransitive` or indeed `phrasal` so game wise its a special case.

The flow chart for the __LOOK__ command is surprisingly simple:

![Local Image](file:///Users/tims/desktop/LOOK_CMD.jpg)

This could again be broken down further but right now its left as above.

## KICK (transitive)
`kick, [the], <thing>, [(at, [the], <thing>)]`
"kick the ball at the window"
	"kick ball at window"
		"kick the ball at window"
			"kick ball"
```ebnf
k_cmd = kick, [the], obj, [(at, [the], obj)];
kick = "kick";
```

## USE (transitive) (compound)
use, [the], obj, [(on, [the], obj)]
"use the key", "use the dynamite"

for now we are not handling this in favour of more direct things like "unlock door" or "unlock door with key"
this will be added with the `compound verbs`

## BURN (transitive)
burn, [the], obj, [(with, [the], obj)]

we could use "set fire to <thing>" but it's easier to use "light <thing>" or "burn <thing>"

* "burn thing"
* "burn the thing"
* "burn thing with matches"
* "burn the thing with matches"

```ebnf
cmd = burn, [the], obj, [(with, [the], obj)];
burn = "burn";
```
## ATTACK
```ebnf
cmd = attack, [the], obj, [(with, [the], obj)];
attack = "attack";
```
## THROW
```ebnf
cmd = throw, [the], obj, [(with, [the], obj)];
throw = "throw";
```
## HIT
```ebnf
cmd = hit, [the], obj, [(with, [the], obj)];
hit = "hit";
```
## LOOT
```ebnf
cmd = loot, [the], obj;
loot = "loot";
```
## LOCK/UNLOCK
```ebnf
cmd = lock | unlock, [the], obj, [(with, [the], obj)];
lock = "lock";
unlock = "unlock";
```
## The General Form
from the break down above we see that most command strings are going to follow the general form:
``` do thing to thing```
and as such we dont need any special handlers for cases other than `LOOK` and `MOVE` actions.



