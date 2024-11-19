### general idea of parsing

we can build up a world using a series of types. in MUD terms `tables`.
to get further functionality we attach further types. 

to define if an action or a verb is possible we first do a parse of the user input which will be (if it has any meaning for us) consist of a `VERB` a `DIRECT_OBJECT` and action dependent an `INDIRECT_OBJECT`.
e.g. `throw the bottle at the wall`, `nick cigs from the magic toad` etc.

for our purposes we divide the `VERBS` we understand into `movement`, `seeing` and `action` types. we further treat an input of a `DIRECTION` as a `VERB` of
`movement` type.

the main routine's parser cares about validity. in our case: is there
a `VERB`? and if that verb is not a `DIRECTION` (`north`, `south`, ...) then have we got a reasonable amount of tokens? i.e. `tokens.length > 1`. 

there is another sanity check, is the length of the input `< 16` as we assume you don't actually have anything useful to say if it takes that long.

in the case of a valid `VERB` we pass it off to either a `movement`, `look` handler or `action` handler.

`LOOK` is a special case which is responsible for building up the view for the players, we dont deal in 3d graphics we deal in 3d textOvision!

the system (@12/04/24) consists of a main parser that checks for length and general validity then forks off to either the `MOVE`, `LOOK` or `ACTION` handlers.

Both `MOVE` and `LOOK` do some further lexing for tokens they care about.

`ACTION`'s are lexed in a general lexer (that we probably should back port into the `LOOK` and `MOVE` systems).

This action lexer returns a struct with and enum for the actual VERB type and then the 2 nouns needed for the rest of the action, the D_OBJ and the I_OBJ,
this then passes to the general action handler.

we can also handle for composite commands in this manner. this is discussed further in the note here [[Composite Commands]]

### general idea of action handling
mainly we only really need the first and the last token to figure out many commands because in english at least there's a fair chance the last token will be the `DIRECT` object. were we smart we would just read up on grammar and sentence structure in english but we are not. also see lazy.
we can tell if we need a `INDIRECT` object as well mainly by the `VERB`.

fishing the `INDIRECT` object is largely a matter of checking the tokens strings length and then checking for prepositions "at" and the definite article "the". there is probably a better pattern? anyway see lazy above.

these tokens are fished out of the command strings and then returned to the handler. each room has an array of objects and an array of direction objects, this distinction is a little hooky, briefly directionObjects are analogous to doors. Objects are objects.

all these Objects have a nested set of properties and arrays or references to other objects and thus we can drill through a set of connected properties looking for matches we care about

### general idea of meta data
we have divided up the `diegetics` (which sounds super cool) of the world into logically `things` and `places` they can both do things to each other. places for example might be incredibly boring, so boring that unless you have some magic glue you literally lose the will to live. or perhaps the path for some reason is made entirely from goat flesh and covers things in blood flavoured milk.

so we chain the aforementioned tables together. so places or rooms have meta data, for example: 
```js
	RoomStore: {
            keySchema: {
                roomId: "uint32",
            },
            valueSchema: {
                roomType: "RoomType",
                txtDefId: "bytes32",
                description: "string", //temp
                objectIds: "uint32[]",
                dirObjIds: "uint32[]",
                players: "uint32[]"
            },
        }
```
this meta data is fetched via the data types id. that way we can drill down into things and decide the outcomes of given verbs. this is true for all objects in the world.

### general idea of descriptions and meta data #look
we give all objects a `description` `string` property. we use this as a simple
description, a `DOOR` is (in terms of its `description`) `a door`. we can then use this to build up description strings from the objects `description`. its `masterialType` and then its `txtDefId` which has a nice verbose description attached. so we can then return a short description or a long description when a `LOOK` command is issued.

### moving around #move
we attach a `direction` object to a `room` this object is really a `struct` returned via a lookup into one of `MUD`'s tables residing in the `store`. This is handled via an id in the room store and then we can drill into this.

so then a direction object itself has a direction type (all the direction's we can go in, north, up, down etc.) and it itself stores metadata which we iterate through and check, things like is it open, is it locked and closed. That determines the movement possibilities and given it is possible to actually move in that manner/direction we then fetch the linked room id and move. Move in this sense means setting the current roomId on the player object.

### puzzle handling #actions

essentially it's the same as everything else, we set properties on the object chain. laboriously.

a good example of the current state of this (@10/04/24) is smashing a window to move into another room.

a window is a direction object and it has a breakable property set. So if one has an object that has this breakable property as well one can break the window.

So far we have only one object really that has this, the football and that requires kicking AT the window (were one to just "kick ball" a general action handler for the kick VRB would be triggered). 
BUT if you were to issue a command with the right direct and indirect object, in this case then "kick ball at window" we would parse down the object tree and see that the window can be broken by the ball and then the broken window's direction object becomes open and thus a move command works.

this should theoretically allow for more complex puzzles to be build, opening locks with keys, burning things etc.

## VERBS are PHYSICS #actions