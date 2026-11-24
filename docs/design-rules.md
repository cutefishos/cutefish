# Design Rules

Layout rules for every Cutefish surface: shell, Settings, applications and
toolkit controls. Sizes are in logical pixels unless they say device pixels.

## Restraint

Each page is one clear thing with one hierarchy. Use one accent and add no
decoration of your own: no glows, badges, illustrations or effects that the
toolkit's controls do not already draw.

## Pixel grid

At fractional scales an odd logical coordinate falls between device pixels.
Every edge lands on a whole device pixel:

- Draw through `Painter`, which rounds each edge to the device grid at its
  absolute position, so boxes that share a logical edge share a device edge.
  Rounded shapes and icons snap their first edge and keep their size.
- Borders, separators, insets and hairlines are whole device pixels, at least
  one. Use `drawRoundedRect`, `strokeRoundedRect`, `drawHorizontalLine` and
  `drawVerticalLine`, and `ui::Icon` for icons, never an antialiased rect at a
  fractional position.
- Text sits on whole device pixels vertically.
- Center content in its container by rounding each half on its own, so the two
  sides differ by at most one device pixel.

Layout tests check these at scales 1, 1.75 and 2.

## Color

Colors come from the theme's `ColorId` roles, which define the light and dark
value of each role once; do not pick RGB values in a view. The accent is
`Highlight`, the color the user chose in Settings. Destructive actions use the
theme's red.

Hairlines:

- A separator between rows is one device pixel of `ColorId::Separator`, inset
  at both ends by the card's horizontal padding.
- The edge under a header and beside a sidebar is `ColorId::HeaderSeparator`,
  half a logical pixel thick: one device pixel with its alpha scaled down below
  2x.

## Corners

Use the theme's radii: `smallRadius` 8 for fields and combo boxes,
`mediumRadius` 10 for buttons and sidebar selections, `bigRadius` 12 for cards,
`windowRadius` 14 for windows and menus.

Continuous corners suit large cards and panels, whose radius is small next to
their size. A pill or a control whose radius is close to half its height keeps
a circular curve; a continuous one flattens it into a lumpy square.

## Insets in a rounded container

An element inset into a rounded window, panel or card, such as a hover or
selection background, a tile or a footer row:

1. Keeps the same gap to every container edge it touches.
2. Takes the radius `container radius - gap`, so its corners stay concentric
   with the container's. Continuous corners are subtracted the same way; a
   `RoundedShape` with an `inset` draws the parallel curve. When the result is
   zero or less, use the control's own radius.

The background grows to meet the gap; text and icons stay at the container's
content padding.

For example, the Wi-Fi, Sound and Battery panels have a radius of 16. Their row
backgrounds sit 6 from the sides, so their radius is 10, while row text stays
16 from the sides. A menu has a radius of 14 and a margin of 5, so its
highlight has a radius of 9.

## Cards and rows

- A card has a radius of 12, a horizontal padding of 16 and a vertical padding
  of 12. Cards stand 12 apart.
- A section label or footnote sits 8 from its card and shares the card's
  horizontal padding, so every text edge on a page lines up.
- A row is 44 high, 52 when it holds a slider or another control taller than a
  switch, and 56 with a second line of text.

## Dialogs

- A titled dialog is a `ui::DialogWindow`. Its title is 12 px, centered on the
  window and on the window controls' line. Do not build a header by hand.
- A sub-page, such as the IP settings of a network, replaces the content in the
  same window and shows the back button. It never opens a second window.
- The content keeps 16 from the dialog's edges, and blocks stand 16 apart.
- Footer buttons stand 8 apart. The primary button is at the end; a
  destructive or extra button is at the start.
- An alert without a title bar is a `ui::ConfirmDialog`: Cancel at the start,
  the confirming button at the end, the confirming button on top when they
  stack.

## Menus and popups

- Menus share one set of metrics: rows 26, separators 12, a margin of 5 on
  every side, text starting at 17 and a minimum width of 168. Do not change
  them per menu.
- A menu button on the right of a title bar opens its menu directly under the
  button, its leading edge on the button's plate (`Menu::popupFor`). Other
  menus open at the pointer.
- Menus, panels and tooltips keep 6 from the bar they open from and from the
  screen edge.

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

## Tab bars

When tabs overflow, both ends of the bar stay round pills and tabs scrolling
past are clipped by the pill's curve, never cut flat. There is no edge fade,
and wheel scrolling moves freely without snapping to tab edges. A short title
keeps 12 from the tab's sides, a long one 28, and hovering never changes how a
title is elided.
