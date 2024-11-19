## verbs
#todo
verbs such as `pick up` and `set fire` can be handled in the same manner as the general case where we look for the `verb` which will always be at the start of the sentence and then hit the last token.
e.g. `pick up the football with the rusty pliers`

the logic for parsing as explained in [[actions_VERBS]] should function almost the same.

given a parse as ![Local Image](file:///Users/tims/desktop/trans_verb.jpg) then we need to handle for `the`

## nouns

these will need some form of mapping as well and as such an amendment to the parser to do a look behind or something similar eg a `rusty axe` 
