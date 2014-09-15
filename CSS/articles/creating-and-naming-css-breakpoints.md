# Creating and Naming CSS breakpoints
This article will address the issues of determining where breakpoints should fall in your responsive implementation and naming conventions around those points. The article assumes completion of a 'desktop' view and the use of a CSS pre-processor (LESS in this case).

## Issue
Determining breakpoints for a responsive implementation is a case-by-case process. There are no boilerplate sizes for breakpoints and they're best determined collaboratively between designer and developer by experimenting with different viewports. Because there are no predefined sizes it is overly specific to name these breakpoints by the device for which they are intended. To name them this way is also fundamentally out of line with ["naming CSS classes"](naming-css-classes.md).

## Creating Breakpoints
#### 1. Determining problem areas
Breakpoints should be determined by experimenting with the site in various size viewports and making notes about where specific elements begin to break down. It is ideal to have both developer and designer sensibilities present when determining problem areas.

#### 2. Find common problem points
Consolidating issues into common points will determine how many breakpoints are needed in adapting the website. This will depend largely on your specific design. The end result this step is a set of sizes `EM/PX/REM` where large, general changes must be made to accommodate the design.

## Naming Breakpoints
It is helpful to think of the ["modifier pattern"](naming-css-classes.md) in naming CSS classes when thinking about names for your breakpoints. Names should be visually relevant and somewhat general. You are _describing_ the state of the website at a given size.
`
_Anti-pattern_
	- desktop
	- tablet
	- iPhone

_Pattern_
	- wide
	- full
	- narrow
	- compact
`
## Next
* ["Implementing CSS Media Queries"](implementing-css-media-queries.md)

## Related Reading
* ["Naming CSS Classes"](naming-css-classes.md)

[Back to All CSS Articles](/CSS/overview.md)