# CSS Refresher

This article explores key CSS concepts, including the Box Model, margin collapse behavior, flow layout, display properties, and positioning.

---

## Box Model

The Box Model is one of the most fundamental concepts in CSS. It represents a box that wraps around all HTML elements and consists of the following parts:

1. **Content**: The actual content of the element.
2. **Padding**: The area around the content, considered part of the element.
3. **Border**: The boundary of the element.
4. **Margin**: The area outside the element, not considered part of it due to margin collapse behavior.

**Note**: It is generally not advisable to specify margins in React applications, as they can disrupt the idea of componentization. Margins affect not only an element internally but also adjacent elements.

---

## Margin Collapse

*Adapted from Josh Comeau's CSS course.*

Margin collapse is a behavior in which vertical margins of adjacent elements combine into a single margin.

### Key Characteristics:
1. Only **vertical margins** collapse; horizontal margins do not.
2. Margins collapse only when **adjacent**, including cases where they are separated by borders, padding, empty spaces (due to fixed heights), or scroll containers.
3. Margins are not summed; the **larger margin wins**.
4. **Nesting** does not prevent collapsing.
5. Margins can **collapse in the same direction**.
6. **Multiple margins** can collapse together.
7. Margins can be **negative**, which is useful for creating indentation effects.
8. Margins collapse only when elements are in **flow layout**.

---

## Flow Layout

Flow layout describes the default way in which HTML elements are displayed on a page. 

### Out-of-Flow Elements:
The following are not considered in the normal flow layout:
1. Floated items.
2. Items with `position: absolute` or `position: fixed`.
3. The root element (`<html>`).

---

## Display

The CSS `display` property defines the type of rendering box created for an HTML element.

### Common Display Types:
1. **Block**:
   - Takes up the entire width of the parent element.
   - Starts on a new line.
   - Allows height and width to be specified with effect.
2. **Inline**:
   - Displayed on the same line as adjacent elements.
   - Element size is based on its content; width and height have no effect.
3. **Inline-Block**:
   - Displayed on the same line as adjacent elements.
   - Allows height and width to be specified with effect.
4. **Flex**:
   - A flexible layout box model for arranging child elements.
5. **Grid**:
   - A two-dimensional layout system for arranging items in rows and columns.
6. **None**:
   - Removes the element from the document layout flow.

---

## Position

The CSS `position` property defines how elements are positioned on the page or relative to other elements.

### Positioning Types:
1. **Static** (Default):
   - Elements are displayed according to the flow layout.
2. **Relative**:
   - Elements are displayed according to flow layout but can be offset using `top`, `left`, `bottom`, or `right`.
3. **Absolute**:
   - Positioned relative to the nearest ancestor with a `relative` or `absolute` position.
   - If no such ancestor exists, the position is relative to the root element.
4. **Fixed**:
   - Positioned relative to the browser window.
   - Scrolling does not affect its position.
5. **Sticky**:
   - Positioned based on the user's scroll. It toggles between `relative` and `fixed` depending on the scroll position.
