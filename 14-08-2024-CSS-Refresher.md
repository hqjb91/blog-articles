#### Box Model

The Box Model one of the most fundamental concepts of CSS. It is a model of a box that wraps around all HTML elements.
The Box Model consists of the following parts :
1. Content - The actual content of the element
1. Padding - The area around the content which can be considered as part of the element
1. Border - A demarcation of the element's border
1. Margin - The area around the element which is not considered as part of the element due to the margin collapse behavior

Note : It is not advisible to specify margin in react applications as they break the idea behind the idea of componentization as margins affect not just an element internally but other elements as well.

#### Margin Collapse (Adapted from Josh Comeau's CSS course)

1. Only Vertical margins collapse and horizontal margins do not collapse
1. Only margins of adjacent elements collapse (obviously) but this also includes obstructions by paddings/borders, empty spaces (created by fixed heights), scroll containers i.e. margins must be touching in order to collapse
1. Margins are not summed up but collapsed and the bigger margin wins
1. Nesting doesn't prevent collapsing
1. Margins can collapse in the same direction
1. Multiple margins can collapse
1. Margins can be negative (very useful in creating indentation effects)
1. Margins only collapse when elements are in flow layout

#### Flow Layout

The normal flow layout describes how HTML elements are displayed on a page by default.

All elements are in flow except for :
1. Floated items
1. Items with `position: absolute` / `position: fixed`
1. Root element `<html>`

#### Display

CSS display defines type of rendering box created from HTML.

1. Block
    1. Element takes up the entire width of parent element and starts a new line
    1. Able to specify height and width with effect
1. Inline
    1. Element displayed on the same line
    1. Element size is based on the content, hence width and height has no effect
1. Inline-Block
    1. Element displayed on the same line
    1. However you can set height and width with effect
1. Flex
1. Grid
1. None

#### Position

CSS position defines how HTML elements are positioned on the page or relative to one another.

1. Static (Default)
    1. Elements are displayed according to flow layout
1. Relative
    1. Elements are displayed according to flow layout but
    1. You can specify top/left/bottom/right to offset it from the natural flow layout position
1. Absolute
    1. Elements are displayed according to offset from first positioned parent element
    1. In case of no parent element with relative/absolute positions the position offset is relative to the root HTML element
1. Fixed
    1. Elements are positioned relatived to window
    1. Scrolling will not affect position
1. Sticky
    1. Elements are positioned based on scroll
