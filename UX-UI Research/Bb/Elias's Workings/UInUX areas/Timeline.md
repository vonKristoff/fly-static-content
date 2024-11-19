## Original thoughts of Requirements:
- See what happens over time with world X
- See what happens at specific time with ?-? worlds
- See what happens with ? worlds and how they interact with each othe
## New User Questions
1) Where is this IP coming from? 
2) ~~I have a story idea, when should it happen in the narrative of the world I want to tell it in? ~~
3)  ***What happened to my IP, where was it used?***
4) I want to see the "significant" events in a narrative.

## Ideas
- Hover over timestamp faded lines appear - showing relationships on hover.
- Simple UI, but throw it out the window when it comes to the data.
- Use levels of gray gradients to make it clear where a timeline starts and where it continues? Humans can distinguish those well, but they struggle with their continuity


## Prototypes
### #1
Combining a simple timeline with events and new lines for splits in time like e.g. for time travel. Clicking on a node will highlight it in the graph view
![[Timeline1_GraphBlocks.png]]
The above timeline transferred to the "Back to the future" timeline as an example. 
![[Timeline1_BackToFutureExample.png]]
Compressed to a single line to save space and with lines on hover to replace or add to the graph view.
![[Timeline_HoverRelatedEvents.png]]
Arrows, snaking their way to the next line to better visualize path connections.
![[Timeline1_ConnectionArrows.png]]

### #2
Vertical prototype, allowing for multiple timelines and fork connections to other worlds. Time durations and time scales (faster, slower, reverse) are all possible
![[Timeline2_TimeScaleAndForks.png]]
Starts getting crowded and unreadable with more connections
![[Timeline2_ArrowMess.png]]

### #3
Time based on narrative events, tumbler like with a parent, super and child worlds.
![[Timeline3_NarrativeTime.png]]
Hierarchy represented via breadcrumbs, allowing for easier origin tracing. Furthermore forks are always represented by diamonds and story events via circles.
![[Timeline3_ForkEventDistinctionBreadcrumbs.png]]

### #4
Narrative timeline with layer jumps for every change of existence. Timestamps on the side to symbolize the time range. Line hovering shows greyed out in between timestamps. 
![[Timeline4_TimebarExample.gif]]
### #5
Narrative Timeline with blocks of identical size at the top. Dates are only displayed when a significant or relevant jump appears or if the user hovers over the blocks. A new line indicates a fork at the given narrative point.
![[Timeline5_SameSizeBlocksRealAndNarrativeTime.png]]
At the bottom is a different view that allows to check for fork events based on real/meat time. Switching between these two views should be possible whenever useful.

A sliding cursor can be used to highlight specific events, even though they may happen at different times and talk about different events. Here's an example highlighting the cursors targets in the graph view. One of the nodes is selected and the contents can be previewed in a sidebar.
![[Timeline5_SameSizeBoxCursorGraphSideInfo.png]]