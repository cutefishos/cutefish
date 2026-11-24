# Design rules

Base layout rules for every Cutefish surface: shell, Settings, applications and toolkit controls.

## Insets in a rounded container

An element inset into a rounded window, panel or card (a hover or selection
background, a tile, a footer row) follows two rules:

1. **Equal gap.** It keeps the same gap to every container edge it sits against.
   An element in the bottom corners is as far from the bottom edge as from the
   left and right edges.
2. **Concentric corners.** Its corner radius is the container radius minus that
   gap: `inner radius = outer radius - gap`. The two curves then stay the same
   distance apart all the way round the corner, as the straight edges do.

When the gap reaches the outer radius or goes past it, the formula gives zero
or less; use the control's own radius instead.

The background grows to meet the gap. Text and icons stay where the layout puts
them, at the container's content padding.

Continuous corners follow the same subtraction: draw the inner shape with
continuous corners too, at the reduced radius.

## Scroll bars at rounded corners

A scroll bar never runs into a rounded corner, whether the corner belongs to the
card or plate its scroll view fills or to the window or panel whose bottom edge
it reaches. The track ends stop `ScrollBar::cornerPadding(radius)` in, which is
`radius - thickness / 2 + inset`. The handle's rounded end is then concentric
with the corner and one inset clear of it, the same gap it keeps to the side.
That is 9 for a 12 card, 11 for a 14 window, and 15 for the 18 notification
centre.

- A scroll view that fills a rounded card or plate calls
  `ScrollView::setCornerRadius(radius)`. This clips the content to the corners
  and pads every bar end.
- A scroll view whose bottom edge is the window's or panel's calls
  `ScrollView::setBottomCornerRadius(radius)`. This pads only the bottom ends and
  does not clip.
- A bar placed by hand, as in terminal, uses `ScrollBar::setPadding` with
  `cornerPadding`, less any margin it already keeps above the edge.

Never hand-pick the padding.

## Scroll bars at the window edge

A page's scroll bar sits against the window's right edge, not inside the
content's margin, and a dialog's against the dialog's right edge. The scroll
view spans the full width; the page margins are insets of its content, so cards
and rows keep their margins while the bar keeps only the toolkit's own inset
from the edge. The end paddings above still apply. A list in a side column that
does not reach the window's right edge keeps its bar at the column's trailing
edge. That edge meets the next column's content, not a window corner, so the bar
takes no corner padding at its ends: a sidebar or list column reaching the
window's bottom keeps the default `ScrollBar::inset` there and does not call
`setBottomCornerRadius`. Only a bar along the window's own right edge clears the
window's corners.

Under a header, tab bar or other strip the content scrolls beneath, the track
starts one inset below it:
`setVerticalBarPadding(header + ScrollBar::inset, ...)`. The same goes for a
bottom strip such as Files' search path strip.

## Example

Wi-Fi, Sound and Battery panels
(`cutefish-compositor/src/shell/controlcenter/status_panel.h`): the window
radius is 16 and every row's hover background (Wi-Fi networks, sound outputs,
the settings link) sits 6 from the left and right edges (`kBackgroundMargin`),
the settings link 6 from the bottom edge too, so their radius is 16 - 6 = 10
(`kBackgroundRadius`). Row text stays 16 from the sides.
