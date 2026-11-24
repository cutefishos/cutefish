# Design Rules

Layout rules for every Cutefish surface: shell, Settings, applications and
toolkit controls.

## Insets in a rounded container

An element inset into a rounded window, panel or card, such as a hover or
selection background, a tile or a footer row:

1. Keeps the same gap to every container edge it touches.
2. Takes the radius `container radius - gap`, so its corners stay concentric
   with the container's. Continuous corners are subtracted the same way. When
   the result is zero or less, use the control's own radius.

The background grows to meet the gap; text and icons stay at the container's
content padding.

For example, the Wi-Fi, Sound and Battery panels have a radius of 16. Their row
backgrounds sit 6 from the sides, so their radius is 10, while row text stays
16 from the sides.

## Scroll bars

A scroll bar never runs into a rounded corner. Its ends stop
`ScrollBar::cornerPadding(radius)` in, which keeps the handle's end concentric
with the corner and one inset clear of it. Never pick the padding by hand.

- A scroll view that fills a rounded card or plate calls
  `ScrollView::setCornerRadius(radius)`, which also clips the content.
- A scroll view whose bottom edge is the window's or panel's calls
  `ScrollView::setBottomCornerRadius(radius)`, which pads the bottom ends only.
- A bar placed by hand uses `ScrollBar::setPadding` with `cornerPadding`, less
  any margin it already keeps above the edge.

A page's scroll bar sits against the window's right edge, and a dialog's
against the dialog's: the scroll view spans the full width and the page margins
become insets of its content.

A list in a side column keeps its bar at the column's trailing edge. That edge
meets the next column, not a window corner, so the bar keeps the default
`ScrollBar::inset` at its ends and does not call `setBottomCornerRadius`.

Under a header, tab bar or other strip that content scrolls beneath, the track
starts one inset below it:
`setVerticalBarPadding(header + ScrollBar::inset, ...)`. A bottom strip works
the same way.
