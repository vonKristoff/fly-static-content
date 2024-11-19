# Permissionless Editing

A core feature of blockchain is that there is no permission required to interact with it.

Applying this to game design is challenging and in the context of the Oruggin Trail, we need to think of the potential issues it can cause to author of rooms and their story.

At its core, The Oruggin Trail is a text story engine where anyone can create rooms and objects and links between them.

What happen if a room is edited by someone else ?

What happen if a room leads to a room without exits.

What happen if a room is surrounded by rooms from another author preventing further rooms to be added.

These conflict need to be either avoided completely or disincentivized.

Disencentivation would require payment or stake but introduce further issues related to supply and cost.

Let's look at the possibility to entirely prevent abuse directly instead

## Avoiding room conflict by design

### Oruggin Trail engine

In Oruggin Trail, there are only rooms and objects. links between rooms are also objects (path, doors, etc...).

And there is no restriction on what those links are. While North, East, South, West are a common set of links, the engine does not care, and rooms that are far away could technically be linked.

In Oruggin Trail, the room exits are thus "portals". Furthermore these portals are only "one way" and if an author wants the portal to be both ways, it need to have such object in both rooms.

### Let anyone add rooms anywhere

Since portal can link room from anywhere to anywhere, it is not possible to block any rooms.

This should thus let us allow players to add rooms anywhere in the world and we don't need to restrict them in that regard.

Now without further restriction, player will also be able to links to existing rooms. This can break the story design of a previous author who designed a specific room to only be accessible under certain condition.

### open vs closed room

As such room should be declared open or close, Open room allow anyone to create rooms that connect to it. While close room prevent other author to connect to it. A typical open room is the starting room of an author's story and usually we can expect the end room to be only accessible from the story and its objects.

We could also imagine having rooms that are closed under certain conidtion only. A Smart contract could be in charge of having the room open or not. From an author point of view, these rooms would be different starting points.

### neighborood

Since portals can link to rooms anywhere there is really no concept of location in The Oruggin Trail but authors can still design directional exit in meaningfullway if they desire. moving to the east and then to the north and then to the west and then to the south could lead back to the first room but this is up to the author.

Other authors that want to links to existing room using directional semantics will need to pay attention to the current room relationship if they want it to make sense, but again it would be up to the author.

Note that this flexibility is what allows anyone to add any room without breaking any rules.

### rooms editing

allowing the editing of other's room is more problematic than letting other add rooms to the world.

This is a problem similar to allowing other to connect to other rooms as adding object can break the intended story of a particular room or set of rooms.

But similarly to room being closed or open, we could also imagine certain author wanting to openup their room for editing under certain condition.

This is out of scope for this current document.

## Conclusion

Adding rooms to world seems to cause little issues (except for [open vs closed room](#open-vs-closed-room) ).

Editing rooms seems more problematic but also less desired.

As such, the Oruggin trail as designed should already accomodate for permission-less editing. We would just need to add the open-close room property and restrict room editing until further thoughts are put into letting other authors edit existing room's and their objects.
