# spacer

a **spacer** is a one-dimensional, discrete space made of cells, where each
cell is in one of two states.

    ■ ■ ◇ ◇ ◇ ■ ◇ ◇

- **one-dimensional**: the cells lie along a single dimension. from any cell
  you can only step one way or the other
- **discrete**: it's made of separate cells, with no positions in between
- **two states**: each cell is in exactly one of them, drawn here as ■ and ◇

the picture runs left to right, but that's only one way to draw it: a spacer
isn't horizontal, or in time, or laid out in any particular way.

a spacer is one kind of discrete space: one dimension, with each cell in one of
two states. everything in this doc is worked out for one dimension, where much
of it is close to self-evident. spaces can have more dimensions, and their cells
can hold other kinds of values. but in more dimensions, things like neighbours,
distance and "the next point" stop being obvious, and many ideas here may
change shape. those paths are noted and deliberately left for later. one other
kind of value comes up with weights.

## circular and linear

a spacer can be read in two ways. the cells and their states stay the same;
what changes is which cells are neighbours, and what a cell's position means.

    cell   0 1 2 3 4 5 6 7
           ■ ■ ◇ ◇ ◇ ■ ◇ ◇

- **circular**: the last cell is next to the first. there are no ends and
  every cell is alike: cell 0 is just where we start counting. a position is
  a place on the circle, and between two cells there are two distances, one
  each way round. what makes it circular is that the last cell meets the
  first; a circle is just the simplest shape that shows it, with every step
  the same size
- **linear**: the first and last cells are the ends. a position is an amount:
  how far from the start. between two cells there is one distance

a reading can also be left open: such a spacer is **fluid**, and can be read
either way later. once a spacer is read circular or linear, though, that
becomes part of what it is: it doesn't go back to fluid, or switch to the
other reading, on its own. turning one into the other takes an operation (see
"operations"): unwinding a circle from its cell 0 gives a line, and winding a
line, so its ends join, gives a circle.

## filled and empty

a cell's two states are called **filled** (■) and **empty** (◇). in other
settings the same pair shows up as 1 and 0, or true and false.

which state is which is a choice, but not an arbitrary one: filled cells are
the things being spaced, and empty cells are the space between them.

- **resolution**: how many cells a spacer has
- **density**: how many of them are filled
- **sparsity**: how many are empty

      ■ ■ ◇ ◇ ◇ ■ ◇ ◇     resolution 8, density 3, sparsity 5

density and sparsity always add up to the resolution.

these three say how many cells of each kind there are, not where they are,
so many spacers share the same three. with 4 cells, 2 of them filled, there
are six:

    ■ ■ ◇ ◇    ■ ◇ ■ ◇    ■ ◇ ◇ ■    ◇ ■ ■ ◇    ◇ ■ ◇ ■    ◇ ◇ ■ ■

swap every cell's state and you get the spacer's **negative**: the same
arrangement/distribution, seen from its empty space. the negative's density is
the original's sparsity. one determines the other, but they aren't the same
thing: a spacer and its negative are two different spacers.

## steps and distances

a **step** moves from a cell to a neighbouring cell. there are two directions
to step in:

- **+**: towards higher cell numbers; drawn to the right, or clockwise. when
  no sign is written, + is meant
- **−**: towards lower cell numbers; drawn to the left, or counter-clockwise

like the numbering itself, which way is + is a convention, and so are right
and clockwise: they're just how + is usually drawn.

the views differ at the ends:

- **circular**: there are no ends, so stepping wraps round. one step − from
  cell 0 is the last cell; one step + from the last cell is cell 0
- **linear**: the ends stop you. there's no step − from the first cell, and no
  step + from the last
- **fluid**: stepping works between the ends; what happens past them is
  left open until a view is chosen

as far as a linear spacer is concerned, nothing exists beyond its ends: it's
**bounded**. sometimes, though, a linear spacer is **scoped**: a piece of
something larger, like a stretch of an endless line. the cells beyond its ends
do exist, just not in this spacer. it's still bounded as a spacer, since its
cells don't record what lies beyond them; the larger whole is context, like an
origin.

the **distance** from one cell to another is how many steps it takes, and it's
never negative; a sign only says which way the steps go. in the linear view
there's one way to go: from cell 0 to cell 5 is +5, and back is −5. in the
circular view you can go either way round: at resolution 8, from cell 0 to
cell 5 is +5 or −3, and the two distances, 5 and 3, add up to the resolution.
unless a sign or the word "shortest" says otherwise, the distance means the +
one: 5 here. the shorter of the two is just as valid, but it has to be asked
for.

### intervals and gaps

the **points** of a spacer are the positions of its filled cells. between one
point and the next, going +, there are two things to measure:

- the **interval**: the distance from one point to the next
- the **gap**: how many empty cells lie between them

an interval is always one more than its gap: the gap, plus the step onto the
next point.

    cell        0 1 2 3 4 5 6 7
                ■ ■ ◇ ◇ ◇ ■ ◇ ◇       points 0, 1, 5

    circular    intervals  1 4 3      add up to the resolution, 8
                gaps       0 3 2      add up to the sparsity, 5

    linear      intervals  1 4        add up to the distance from the first point to the last, 5
                gaps       0 3        plus end gaps of 0 and 2; all together, the sparsity, 5

- **circular**: one interval and one gap per point. with a single point, the
  interval goes all the way round, the whole resolution
- **linear**: one interval and one gap fewer than there are points, plus an
  **end gap** at each end: the empty cells before the first point and after
  the last. in a scoped spacer, the end gaps may be cut short: the empty cells
  can carry on past the ends
- an all-empty spacer has no points, so no intervals and no gaps. in an
  all-filled spacer every interval is 1 and every gap is 0

(in number theory, a "prime gap" is the difference between neighbouring
primes: here, that's an interval, not a gap.)

### seams, bands and runs

every place where two neighbouring cells meet is a **joint**: a line of n
cells has n − 1 joints, and a circle of n cells has n, since its last cell and
cell 0 meet too. a **seam** is a joint where the two cells are in different
states. the seams cut a spacer into **bands**: stretches of cells in the same state, each
as long as it goes. a band of two or more cells is a **run**. (**panel** would
work as well as band: panels are what seams join.)

    cell      0 1 2 3 4 5 6 7
              ■ ■ ◇ ◇ ◇ ■ ◇ ◇
    bands     ■ ■ | ◇ ◇ ◇ | ■ | ◇ ◇       4 bands, 3 of them runs

- **linear**: `<xx...x..>` has 3 seams and 4 bands, so one more band than seams
- **circular**: the last and first cells are neighbours too, so there can be a
  seam between them: `)xx...x..(` has 4 seams and 4 bands. a band can also
  carry on past the last cell into the first: `)x...xx(` has 2 seams and 2
  bands, `[xxx]` (cells 4, 5, 0) and `[...]`, both runs
- an all-empty or all-filled spacer has no seams: one band covering
  everything

bands follow from the gaps: a gap of 0 joins two points into the same filled
band, and any other gap is an empty band.

### whole numbers

everything counted or measured in a spacer is a whole number. counts and
distances are never negative; a sign only gives a direction. resolution is at
least 1, density and sparsity run from 0 to the resolution, and cells are
numbered from 0 to one less than the resolution. a number outside that range
doesn't name a new cell: in the circular view it wraps round (cell −1 is the
last cell); in the linear view there's no cell there. proportions, like "half
the cells", only come in with generators.

## any arrangement

a spacer puts no condition on which of its cells are filled: any
arrangement/distribution of filled and empty cells is a spacer, whether it
follows a clear pattern, has some structure, or has no rhyme or reason.

    ■ ■ ◇ ◇ ■ ■ ◇ ◇     a repeating pattern
    ■ ■ ■ ◇ ◇ ◇ ◇ ◇     one block
    ■ ■ ◇ ◇ ◇ ■ ◇ ◇     no particular pattern

so two things need keeping apart:

- **the spacer as a device**: open-ended. it only says there are cells, each
  filled or empty, and it holds any arrangement at all
- **the meaning of an arrangement**: whether a particular one follows a rule,
  and where it came from (a rhythm, a set of numbers, a coin toss). that
  belongs to the arrangement, not to spacers in general

the cells don't record where an arrangement came from: the same cells could
come from a careful rule or from tossing a coin, and nothing in them would show
which.

put another way: a filled cell is just filled. it carries no metadata about
why it's filled, or what reason/origin put it there. whatever an arrangement
means lives outside the spacer, in what we know about where it came from.

later chapters look closely at particular kinds of arrangement; each is one
kind among all spacers, not what a spacer is.

## the smallest spacers

a spacer has at least one cell. the smallest ones are worth a look, because
they're where the general rules reach their limits.

- **all empty** and **all filled**: every cell in the same state. both are
  spacers, and each is the other's negative
- **one cell**: in the circular view the cell is its own neighbour, both ways
  round; in the linear view it's both ends at once
- **two cells**: in the circular view the two cells are neighbours twice, once
  each way round. from three cells on, every cell has two different neighbours

### zero

with no cells at all there's no space, and so no spacer. we still give it a
name, **zero**, because it can come up as the result of a change that leaves
no cells. zero isn't the all-empty spacer: an all-empty spacer is a space with
nothing in it; zero is no space at all.

## writing a spacer down

a spacer is written in one of two forms, which map one to one:

- **typed/keyboard**: `x` for filled and `.` for empty, with no spaces, in
  order from cell 0. easy to enter by hand
- **display/read-only**: shapes, with a space between cells. purer to read

anyone, or any program, can write either; the typed form is simply easier on a
keyboard.

### rules for the symbols

other symbols work just as well, as long as they follow five rules:

1. **distinct states**: filled and empty look different
2. **separate cells**: every cell can be told apart from its neighbours
3. **countable runs**: a run of cells in the same state doesn't blur
   together; you can count the cells in it at a glance
4. **minimize collisions**: keep the symbols' other meanings from getting in
   the way. be pragmatic about it: what collides depends on the context the
   notation is read in, and no symbol is free of other meanings, so this is a
   judgement, not a ban
5. **clear edges**: it's clear where a written spacer starts and ends

`|xx___x__|` breaks rule 3: in many fonts the underscores merge into one line,
so it's hard to tell how many empty cells there are.

rule 4 is the subtle one. 0 and 1 are also numbers, so `1 0 1 0 0 1 0 0` could
be eight states, or eight amounts that happen to be 0 or 1, and the symbols
alone can't say which. later chapters do produce rows of amounts like that, so
here digits are a poor choice for states, and a row of digits always means
amounts.

no pair of symbols is entirely free of other meanings: `x` is also a letter,
and `.` a full stop. in a run they don't read as anything else, but a single
`x` could. that's what rule 5 is for: in the typed form a spacer always has its
edges, so `|x|` is clearly a spacer of one filled cell. (in figures, where a
spacer sits on its own line with labels, the layout marks its edges.)

### edges show the view

in the typed form, the edges around a spacer say how it's read:

    )xx...x..(    circular      round brackets turned outward: the edges open
                                onto each other, as if the ends join round the back
    <xx...x..>    linear        bounded: an end on each side, and nothing beyond
    [xx...x..]    scoped        a piece of a larger spacer: it has ends, but
                                there's more beyond them
    |xx...x..|    fluid         plain walls: just the spacer, no reading chosen

**zero** is `|><|`: the walls pushed together, with no space left between them.
it only takes the plain walls, since with no cells there's nothing to read as a
circle or a line, and nothing inside could be a cell, so it doesn't read as a
spacer. `>…<` isn't used, so zero's middle can't be mistaken for edges turned
inward.

### the display form

in the display form, the shapes of the cells carry the view, so no edges are
needed. each shape is its typed edges closed up into every cell, and filled
and empty always differ twice: in fill, and in size, direction or turn:

    )xx...x..(    ● ● ⚬ ⚬ ⚬ ● ⚬ ⚬         circles, from round brackets: big filled,
                                          small empty
    <xx...x..>    ▲ ▲ ▽ ▽ ▽ ▲ ▽ ▽         triangles, from < and > turned upright:
                                          filled up, empty down
    |xx...x..|    ■ ■ ◇ ◇ ◇ ■ ◇ ◇         squares, from straight walls: filled
                                          square, empty square turned (a diamond)
    [xx...x..]    [ ▲ ▲ ▽ ▽ ▽ ▲ ▽ ▽ ]     a piece is linear in its own terms;
                                          the brackets say there's more beyond
    |><|          |▶◀|                    zero

a single ● can't be mistaken for anything else, so the shapes do the job the
edges do in the typed form. the figures before this section use the fluid
shapes: no view chosen yet. where no cells are drawn (in points, labels and scopes,
below), the edges stay, with → and − in place of -> and -. square brackets
around cells get a space inside them, like the cells do: `[ ▲ ▽ ▲ ]`. ranges
and other numbers stay as they are: `[5→8]`.

the display form needs geometric shapes (● ⚬ ▲ ▽ ■ ◇), which most modern fonts
include. some terminals give them two columns instead of one, which can upset
alignment; ⚬ is drawn narrower than the others there, so circular rows are the
first to go out of line. where they aren't available, write the typed form, or
any other symbols that follow the five rules: the shapes are one read-optimized
choice, not the only one.

(a tangent: having filled and empty differ twice spends a second visual
difference, size, direction or turn, that could otherwise carry something
else. if a later use needs to show an extra property of each cell, that
second difference is the first thing to give back.)

### points

a spacer can also be written by its points, after its resolution:

    |xx...x..|   →   |8: 0,1,5|
    )x..x.(      →   )5: 0,3(

the resolution has to be there: without it, `|x..x.|` and `|x..x...|` would
both come out as `0,3`. an all-empty spacer has no points: `|4:|`. here the
digits are positions, which are numbers, so rule 4 holds. the point form is
shorter when few cells are filled; the cell form shows the arrangement at a
glance.

### labels

a spacer can be given a name, a **label**, to refer to it later:

    S := |x.xx..|

`:=` means "is defined as", which keeps `=` free for saying two things are the
same. labels start with an uppercase letter and are never `X`, so they can't be
mistaken for cells; words work too, like `Primes` or `S1`. inside edges, a
label stands for its cells: `)S(` is S read circularly. a reading can only be
applied to a fluid label: if S is already circular, `)S(` just repeats its
reading, and `<S>` isn't allowed. like the point form, a label can carry its
resolution: `)6: S(`. a label is defined once: it names one value, and never
changes to mean another.

### descriptions

a **description** lists what's known about a spacer, in braces:

    {r: 12, d: 7}         resolution 12, density 7
    S{r: 12, d: 7}        S: some spacer with resolution 12 and density 7

on its own, a description picks out every spacer that fits it: `{r: 12, d: 7}`
fits 792 different spacers, not one. keys can be full words (`resolution: 12`)
or short ones, but a short key, like a short label, is only used once it's been
mapped to its full name. so far: `r` for resolution, `d` for density and `s`
for sparsity. every number comes with its key, so nothing has to be guessed
from its position.

### repetition

outside the edges is context. `~` says a spacer repeats, out there:

    )x..x.(~       repeats without end
    )x..x.(~3      repeats 3 times

it describes the surroundings and doesn't change the spacer: its resolution is
still 5. the study of strings writes repetition as a power, w³ for w three
times over, but that *builds* a new, longer string; here `^` would mean
building it.

### scopes

a **scope** takes a piece of a spacer. it's written after the spacer, in square
brackets, and the spacer's edges and resolution always come with it, because
how a scope behaves depends on both:

    S := |x.xx..|

    )6: S([5+3]      →  [.x.x]      from cell 5, 3 steps +
    )6: S([5->8]     →  [.x.x]      from cell 5 to cell 8, written in full: the same cells
    )6: S(-[5+3]     →  [..xx]      a − in front walks the other way: cells 5, 4, 3, 2
    <6: S>[2->5]     →  [xx..]      read linearly: cells 2 to 5
    <6: S>[5+3]      →  no piece    read linearly, S is bounded, and this runs past its end

- **relative**, `[start+steps]`: the steps go +. the start and end cells are
  both included, so 3 steps cover 4 cells
- **absolute**, `[start->end]`: always goes +. the end is written in full, not
  wrapped round: 8 at resolution 6 shows the walk passed the last cell and came
  round to cell 2
- **direction**: a − in front of the brackets walks the other way, and the
  piece is written in the order it was walked. the direction sign only ever
  sits outside the brackets, so a − inside them always means a position
  counted back from cell 0 (circular only): `)8: S([-2->2]` runs from two
  cells before cell 0 to two after it
- **laps**: in a circular spacer, `~3%` means three full laps, without having
  to state the resolution. `)6: S([2~3%+5]` starts at cell 2, goes round 3
  times, then 5 more steps: 23 steps in all, or `[2->25]` as an absolute range.
  `%` is the programmer's wrap-around (modulo), so read `3%` as "3 laps", not
  "3 percent"
- **fluid**: in `|6: S|[…]`, what lies past the ends is undecided, so a
  scope has to stay within them
- **the result** is a scoped piece, `[…]`, with its own numbering: its cell 0
  is where the scope started, and its + runs the way the scope walked

### scopes of scopes

a scoped piece can be scoped again, using its own numbering: its cell 0 is
where it started.

    S := )xx...x..(
    A := )8: S([5+4]       →  [x..xx]      S's cells 5, 6, 7, 0, 1
    B := [5: A][2->4]      →  [.xx]        A's cells 2, 3, 4

the same thing can be written as a **chain**, where each bracket counts in the
piece the one before it made:

    )8: S([5+4][2->4]      →  [.xx]

a piece of a piece follows a few rules:

1. **it stays inside its parent**: it can't reach past the parent's first or
   last cell. `[5: A][3+4]` runs past A's end, so there's no piece
2. **no laps**: a scoped piece has ends, so there's no way round it. `~3%`
   only works on a circular spacer
3. **the edges stay square**: a piece is always read as `[…]`. joining its
   ends to read it circularly would make a new spacer, which is a change, not
   a scope
4. **no repetition**: what lies beyond a piece is its parent, not copies of
   itself, so `~` doesn't apply
5. **everything else works as usual**: relative and absolute ranges, as long
   as the end stays inside the piece, and `-` for walking the other way

**back to the root.** because each piece stays inside its parent, any piece of
a piece is one unbroken stretch of the spacer the chain was first taken from.
usually that's the **root**: the spacer at the very top, which isn't a piece or
a layer of anything. if it's a layer (see "layers"), the layer is the first
piece's parent, and the chain can't look past it. to find where a piece sits,
go up one parent at a time: add the parent's start to the piece's start. if the
parent walked −, subtract instead, and flip the direction. you can stop at any
parent on the way up. a start can come out below 0, which can only happen when
the chain starts in a circle: at resolution 8, −2 and 6 name the same cell, but
−2 keeps how it was reached. either can be written.

    B starts at A's cell 2, and A starts at S's cell 5:   5 + 2 = 7
    B in S:   )8: S([7+2]  =  )8: S([7->9]          →  [.xx]

    C := )8: S(-[1+4]      →  [xx..x]      S's cells 1, 0, 7, 6, 5
    D := [5: C][1->3]      →  [x..]        C's cells 1, 2, 3
    D in S: C walked −, so 1 − 1 = 0, going −:   )8: S(-[0+2]   →  [x..]

the chain records how a piece was made; written as a scope of the root, it
records where it ends up. both show the same cells, but they're mounted on
different parents (see lift and collapse, in "operations").

### inside other formats

the notation is defined on its own terms. inside another format that already
uses some of its symbols, it's escaped the way that format asks, never changed
to suit it. in Markdown, for example, `|` splits table columns (typed `\|`
there), `*` and `~` can turn text bold or struck through, and a bare `<x>` can
vanish as an HTML tag. inside code formatting, most of this goes away.

### rotating

a written spacer always has a first cell, even in the circular view, where
it's simply cell 0. to start from another cell, rotate it so that cell comes
first: starting `)xx...x..(` at cell 5 gives `)x..xx...(`; in points,
`)8: 0,1,5(` gives `)8: 0,3,4(`.

## why not "binary string"

write each cell's state as 1 for filled and 0 for empty and you get a binary
string:

    ■ ■ ◇ ◇ ◇ ■ ◇ ◇   →   11000100

it's a familiar way to write something like a spacer, and it follows the first
three rules above, though not the fourth. we don't call a spacer a binary
string, and we don't write it with digits, for three reasons.

**the name should match the object's affordance.** an affordance is what an
object lets you do, and a spacer lets you space things out: it's about where
things sit and how far apart they are. so it's named after that.

**this encoding is literal; most binary encodings aren't.** in a spacer's
string, each character is the cell at the same position: reading the string
is looking at the spacer, with nothing to translate. binary encodings usually
have a step of translation in between:

- in a **binary number**, position matters, so it's partly geometric. but
  each position stands for a size (1, 2, 4, 8, …), and the string means the
  total they add up to
- in most other uses (text, images or sound stored as bits), the order is a
  **serialization**: chosen for storing or sending, with positions that mean
  nothing until a format decodes them

so "binary string" suggests something to be decoded. a spacer is a discrete
geometric object, and its written form is a literal picture of it. so we use
the language of space (cells, positions, distances), not the language of
encoding (bits, digits, codes).

**digits are needed for other things.** this is rule 4 above: digits stay
numbers. a 0 would also read as zero (no cells at all).

## layers

a spacer's filled cells can become the cells of another spacer. that second
spacer is a **layer**: it chooses among the first one's points. the first
spacer is the **root**, as with scopes.

    cell      0 1 2 3 4 5 6 7
    root      ■ ■ ◇ ◇ ◇ ■ ◇ ◇      points 0, 1, 5
    layer     ■ ◇       ■          its 3 cells are the root's 3 points; it keeps 0 and 5

layers form a **cascade**, which can go deeper: a layer's points can carry a
further layer, and so on.

- **the fitting rule**: a layer's resolution is the density of the layer above
  it. it can only choose among points that are there
- **distance at two scales**: in the layer, its cells 0 and 2 are two steps
  apart, since its steps go from point to point. on the root, the same two
  cells are five steps apart
- **one reading**: every layer is read the way its root is. points round a
  circle stay in circular order; points along a line stay in line order
- **each layer's own conventions**: a layer has its own cell 0 and direction,
  like any spacer
- **zero ends a cascade**: if a layer is all empty, a layer below it would have
  no cells, which is zero. so a cascade stops at its first all-empty layer
- **flattening**: the deepest layer's choices can be shown on the root's
  cells: here, `|x....x..|`. the flattened spacer doesn't show it was chosen in
  steps; like any origin, the cascade lives outside the cells (see press and
  flatten, in "operations")

layers aren't scopes. a scope takes a stretch of neighbouring cells; a layer
chooses among points, skipping the cells in between. both nest and both chain,
and they can be mixed: a scope of a layer is a stretch of consecutive points.

a scope of a layer has the layer as its parent, and can't look past it.
placing it on the cascade's root means following the layers up, which is
flattening, not scoping, and a stretch of consecutive points in a layer can be
scattered across the root: in `|xx...x..| >> |x.x|`, the layer's
cells 1 and 2 are the root's cells 1 and 5.

### writing layers

a cascade can be written from either end. each `>>` or `<<` points at the deeper
layer:

    |xx...x..| >> |x.x|          top-down: the root first, then the layer that chooses from its points
    |x.x| << |xx...x..|          bottom-up: the layer first, placed into the root's points
    |8: 0,1,5| >> |3: 0,2|       point form: the root has 3 points, so the layer has 3 cells
    A >> B >> C                  labels, to any depth; C << B << A is the same cascade

- top-down reads as choosing: start from the root and pick points layer by
  layer. bottom-up reads as placing: start from a small spacer and fit it into
  larger ones
- every layer repeats the root's edges, so a circular cascade is `)…( >> )…(`. a
  cascade can't mix readings
- a chain goes one way only: `A >> B << C` isn't written

in the display form, the root sits at the top, whichever way the cascade is
written, and each layer's cells sit under the points they stand for:

    ■ ■ ◇ ◇ ◇ ■ ◇ ◇
    ■ ◇       ■

### cells that open into spacers

a different kind of nesting, noted here as a concept only: each cell of a
spacer opens into a spacer of its own, the way a tile can be divided into
smaller tiles. it's close to refinement, and it's left for later.

## collections, stacks and weights

### collections

a **collection** is several spacers taken together. two things about a
collection are independent of each other:

- **order**: whether its members come in a set order
- **repeats**: whether the same spacer can appear more than once

each combination has an established name:

|               | no repeats             | repeats allowed            |
| ------------- | ---------------------- | -------------------------- |
| **unordered** | set: `{A, B, C}`       | multiset: `{#A, A, B#}`    |
| **ordered**   | ordered set            | sequence: `(A, A, B)`      |

an ordered set is written like a sequence whose members all differ. the display
form can write a multiset with bag brackets: `⟅A, A, B⟆`.

what counts as a repeat depends on what counts as the same spacer, so, like
identity, whether a collection has repeats is decided by the use. a description
picks out spacers too, and what it gives can be any of these kinds:
`{r: 8, d: 3}` gives every fitting spacer once, but other descriptions can give
repeats, or an order.

### stacks

to count across a collection, its spacers are **stacked**: lined up cell by
cell. stacking is a device for working with spacers, not a new kind of spacer,
and it needs a few things settled:

- every member has the same resolution
- each member sits in a **frame**: the fixed choices that say how it lines up,
  meaning which of its cells is cell 0, which way it runs, and how it's read.
  by default, a spacer is framed as written, with its cell 0 over the stack's
  cell 0
- all members share one reading, and the edges around a stack say which:

      )(x.., .x.)(      circular
      <(x.., .x.)>      linear
      |(x.., .x.)|      fluid
      )(A, B)(          labels too: reads both A and B circularly

  edges around anything apply a reading to it, the way `)S(` reads S
  circularly, as long as what's inside is fluid or already reads that way.
  so the members inside are written bare. for now, a stack can't mix readings

framing is where identity gets decided: a stack takes each spacer exactly as
framed, so `)x.x..(` and `)x..x.(` add to different columns, although read
circularly one is the other started from a different cell.

### weights

    cell      0 1 2 3 4 5 6 7
    A         ■ ■ ◇ ◇ ◇ ■ ◇ ◇
    B         ■ ◇ ◇ ■ ◇ ■ ◇ ◇
    C         ◇ ■ ◇ ◇ ◇ ■ ■ ◇
              ---------------
    weights   2 2 0 1 0 3 1 0

- the **cell weights** count the filled cells in each column. they ignore order
  and count every repeat
- the weights live in a space of their own: the same cells and reading as the
  stack, but each cell holds an amount instead of one of two states, so a row
  of weights isn't a spacer. the reading carries through: circular members give
  circular weights, linear members linear ones, and fluid members leave the
  reading open, to be chosen by the use. writing the members circular or linear
  says the reading is part of the structure; writing them fluid says it's a
  perspective for later
- the weights add up to the members' densities together (9 here), and no
  weight is more than the number of members
- a member's **spacer weight** (or score) is the weights added up at its
  points: how much it agrees with the whole, itself included. A's is
  2 + 2 + 3 = 7; B's and C's are 6
- an ordered stack has **stages**: its running weights as each member goes
  on. unlike the final weights, it depends on the order:

      after A    1 1 0 0 0 1 0 0
      after B    2 1 0 1 0 2 0 0
      after C    2 2 0 1 0 3 1 0

weights are a derived result, and for most stacks they're just that: a count.
but when the spacers going in are patterned, the weights can carry structure of
their own, and become objects worth studying in their own right: a space of
amounts, where the amount works almost like an extra dimension. that world is
noted here, not explored. the weights are derived, but they can show structure
of the whole that no member shows on its own: what's sometimes called weak
emergence.

weights are written as a row of numbers, which are amounts, not states (rule
4): `|2 2 0 1 0 3 1 0|`. `Weights((A, B, C))` gives a stack's weights as such a
row.

the row takes the stack's reading, written with the same edges: circular
members give `)2 2 0 1 0 3 1 0(`, linear ones `<2 2 0 1 0 3 1 0>`, and fluid ones
`|2 2 0 1 0 3 1 0|`. the display form keeps the edges, since no cells are drawn.
for example, `Weights( )(A, B, C)( )` gives `)2 2 0 1 0 3 1 0(`.

the stages are written as a sequence of rows, one per step: `Stages((A, B, C))`
gives `(|1 1 0 0 0 1 0 0|, |2 1 0 1 0 2 0 0|, |2 2 0 1 0 3 1 0|)`. its last row
is the stack's weights.

("weight" has another meaning in coding theory, where a string's weight is its
number of 1s; here, that's density. here, weights are always taken across a
stack: cell weights per cell, and a spacer weight per member.)

## combining spacers

two spacers with the same resolution can be combined cell by cell into a new
spacer. like stacking, this frames both spacers, as written by default, and the
same reading rules apply: both circular, both linear or both fluid, and the
result takes their reading.

    A              ■ ■ ◇ ◇ ◇ ■ ◇ ◇
    B              ■ ◇ ◇ ■ ◇ ■ ◇ ◇
    A and B        ■ ◇ ◇ ◇ ◇ ■ ◇ ◇      filled where both are
    A or B         ■ ■ ◇ ■ ◇ ■ ◇ ◇      filled where at least one is
    A xor B        ◇ ■ ◇ ■ ◇ ◇ ◇ ◇      filled where exactly one is
    A but not B    ◇ ■ ◇ ◇ ◇ ◇ ◇ ◇      filled where A is and B isn't

the negative is the same idea for one spacer: `not A` is filled wherever A
isn't.

these are the familiar operations of logic, and of sets read on the points:
`and` is intersection, `or` is union, `xor` is symmetric difference, `but not`
is difference and `not` is the complement. the typed form writes them as words
between the spacers; the display form can use the set symbols: `A ∩ B`,
`A ∪ B`, `A △ B`, `A ∖ B`. `not`, `and` and `or` are primitives; `but not` and
`xor` are made of them (see "operations").

they're also thresholds on weights: stack A and B, and `A and B` is where the
weight is 2, `A or B` where it's at least 1, and `A xor B` where it's exactly 1.
with more spacers in a stack, any threshold works the same way, such as "at
least half of them" (see threshold, in "operations").

`and` and `or` chain without brackets: `A or B or C` is filled where at least
one is, and `A and B and C` where all are. `xor` chains too, but it doesn't mean
"exactly one": `A xor B xor C` is filled where an odd number are. with the
stack from "collections, stacks and weights", that's `|...x.xx.|`, while
"exactly one" is `|...x..x.|`.

## operations

an **operation** takes spacers and gives back a spacer. they come in two kinds:

- **primitives**, which each do one basic thing: commit, rotate, slide,
  reverse, mirror, join, unwind, wind, detach, lift, press and threshold, along
  with taking a scope, layering (`>>`), and the cell-by-cell `not`, `and` and
  `or` from "combining spacers"
- **named composites**, which are made of primitives and named because they're
  useful: repeat, insert, splice, collapse, flatten, `but not`, `xor`, at most,
  exactly, levels and depths. each is defined by the
  primitives it's made of, so composites can be added or dropped without
  touching anything else

most are written as functions, the way generators take their values: the
operation's name, then its values in round brackets, in a stated order. like
stacking, an operation works on spacers as written, by default.

operations never change a spacer in place: they always give a new one, and
what went in stays as it was.

most operations work on a spacer's cells. a few work on a view instead: they
change how a spacer is looked at, not the spacer itself. taking a scope,
sliding it, lifting it and committing a fluid spacer to a reading are of this
kind. detach and flatten are the bridges from a view, or a cascade, back to a
spacer of its own.

what's allowed depends on the readings going in, by the same idea as stacks:
writing a spacer circular or linear says its reading is part of its structure;
writing it fluid leaves the reading open, so it can take whichever part the
operation needs.

an operation can borrow a reading while it works, as long as just one reading
makes sense for it and its result doesn't depend on it afterwards, and **fluid
in gives fluid out**: committing to a reading is left to whoever uses the
result. a fluid spacer can be rotated as a circle and then read as a line: with
`N := |xx...x..|`, `Rotate(N, 5)` is `|x..xx...|`, and `<Rotate(N, 5)>` is
`<x..xx...>`. the only exceptions are the operations whose job is to change a
reading: unwind and wind.

each operation does one kind of thing to one kind of spacer. when the same idea
means different things for lines and circles, they get different operations,
rather than one operation that behaves differently depending on what it's
given.

### primitives

#### commit

committing gives a fluid spacer a reading, by putting edges around it. it's a
view operation: the cells stay as they are, and the result is a new, committed
spacer. the fluid one stays fluid.

    N := |xx...x..|
    )N(                       →  )xx...x..(       committed circular
    <N>                       →  <xx...x..>       committed linear

a literal can simply be written committed from the start: `)xx...x..(`. edges
can also go around an expression, or around something passed to an operation.
wherever an edge would touch another bracket, a space keeps them apart:
`) Rotate(N, 5) (`, `Rotate( )N(, 5)`.

there are two places to commit, and they say different things:

- **on what goes in**: `Rotate( )N(, 5)` commits N as a circle before rotating
  it, so the result is a circle too: `)x..xx...(`. this says the circle is
  part of the structure. using the result as a line afterwards takes a
  conversion: `Unwind(Rotate( )N(, 5))` gives `<x..xx...>`
- **on what comes out**: `<Rotate(N, 5)>` keeps N fluid through the rotation,
  which borrows a circular reading, and commits only the result, as a line:
  `<x..xx...>`. this says the circle was only needed for the move

both arrive at the same cells here. which to use depends on whether the reading
is part of the structure, or a perspective decided later.

#### rotate

`Rotate(S, k)`, a **rotation/phase**, starts S from its cell k: the cells come
round in the same order, and cell k becomes cell 0. k can be negative: −2 is
two cells back from cell 0.

    S := )xx...x..(
    Rotate(S, 5)       →  )x..xx...(
    Rotate(S, -2)      →  )..xx...x(

- **circular**: yes. this is the rotating described in "writing a spacer down"
- **fluid**: yes, read circularly for the move; the result stays fluid
- **linear**: no. a line has no way round to rotate along. a scoped piece can
  slide along its parent instead

#### slide

`Slide(A, k)` moves a scoped piece along its parent: the window keeps its
length, and starts from what was its cell k, which can lie beyond its end. it
works on the view, not on a spacer: the parent stays as it is, and only the
part the window shows changes.

    S := )xx...x..(
    A := )8: S([5+4]       →  [x..xx]       S's cells 5, 6, 7, 0, 1
    Slide(A, 2)            →  [.xx..]       S's cells 7, 0, 1, 2, 3: )8: S([7+4]
    Slide(A, -1)           →  [.x..x]       S's cells 4, 5, 6, 7, 0

    T := <xx...x..>
    B := <8: T>[2->5]      →  [...x]
    Slide(B, 2)            →  [.x..]        T's cells 4 to 7
    Slide(B, 3)            →  not allowed   it would run past T's end

- it moves k steps the way the window walks, in its own + direction
- **circular parent**: it slides freely, wrapping round
- **linear parent**: it can't pass the parent's ends. sliding only as far as it
  can (clamping), or letting the window shrink, would both change what was
  asked for without saying so
- **fluid parent**: within the ends, it slides as usual. crossing an end would
  only make sense if the parent were circular, and the window stays tied to its
  parent afterwards, so the parent's reading has to be committed first: `)S(`
  to wrap round, `<S>` to stay bounded
- a scope of a scope slides within its immediate parent. lifting it first lets
  it slide within the parent above (see lift)
- it undoes cleanly: `Slide(Slide(A, 2), -2)` is A again, as long as each slide
  was allowed

#### reverse

`Reverse(L)` runs a line's numbering the other way: its ends swap, so
`<xx...x..>` becomes `<..x...xx>`.

- **linear**: yes
- **circular**: no; a circle is mirrored instead
- **fluid**: yes, read as a line for the move; the result stays fluid.
  `Reverse(|xx...x..|)` is `|..x...xx|`, while `Mirror(|xx...x..|)` is
  `|x..x...x|`: two different operations, so nothing is left ambiguous

#### mirror

`Mirror(C)` mirrors a circle, pivoting on cell 0: cell 0 stays where it is, and
cell j goes to cell −j, so the other cells come round the other way.

    )xx...x..(   →  )x..x...x(       8 cells: the mirror line runs through cell 0 and cell 4
    )xx..x..(    →  )x..x..x(        7 cells: through cell 0 and the joint between cells 3 and 4

- **circular**: yes. whether the mirror line comes out at a cell or at a joint
  on the far side depends on whether the resolution is even or odd; the
  operation is the same either way
- **fluid**: yes, read circularly for the move; the result stays fluid
- **linear**: no; that's reverse
- mirroring twice gives the circle back, and mirror and rotate combine the way
  reflections and turns do: `Mirror(Rotate(C, k))` is `Rotate(Mirror(C), -k)`
- any other mirror line is a mirror and then a rotate. the line through the
  joint before cell 0 is `Rotate(Mirror(C), 1)`, which is also
  `Wind(Reverse(Unwind(C)))`: reading the cells backwards and closing them
  again

#### join

`Join(A, B)` puts B after A's last cell: two lines, end to end, make a longer
line.

    Join(<x.>, <.xx>)      →  <x..xx>

- only something with free ends can be joined: a line, or a fluid spacer read
  as one. joining two fluid spacers gives a fluid one; joining a fluid spacer
  to a line gives a line
- a circle written circular says it has no ends, so there's nothing to attach.
  if its circularity is only a perspective, it can be written fluid, and then
  it can be joined
- a scoped piece has ends, but they aren't free: just past each one is its
  parent's next cell, and attaching something there would contradict the
  parent. detach it first

#### unwind and wind

these two change a committed reading, so they're the only operations that turn
a circle into a line, or a line into a circle.

`Unwind(C)` opens a circle into a line, starting from its frame's cell 0.
`Wind(L)` joins a line's two ends into a circle.

    C := )x..(
    Unwind(C)               →  <x..>          the same cells, now as a line
    Wind(<x..>)             →  )x..(          the ends join
    Repeat(Unwind(C), 3)    →  <x..x..x..>    three laps: unwind, then repeat

- each does one thing: unwind only opens, and wind only joins the ends. laps
  come from repeat
- the round trip loses nothing: winding an unwound circle gives it back, and
  unwinding a wound line gives the line back, since both start at cell 0
- unwind takes a circle and wind takes a line: they're the conversions between
  the two committed shapes. a fluid spacer doesn't need them, since it commits
  directly, as `)N(` or `<N>`. this keeps the two moves apart, as the
  comparisons do: committing is fitting (`~=`), and converting is unwind and
  wind (`<->`)
- unwinding starts where the spacer's frame starts, its cell 0 as written;
  starting somewhere else means rotating first
- a circular scope with laps walks the same cells as unwinding and repeating:
  `)3: C([0~2%+2]` gives `[x..x..x..]`, a scoped piece

unwinding can also happen over time: a generator that unwinds one more cell at
each resolution, where cell j copies C's cell j, counting round C as many times
as needed. earlier cells never change, so it grows: `|x|` → `|x.|` → `|x..|` →
`|x..x|` → …, and without end it reaches C repeating: `)x..(~`.

#### detach

`Detach(A)` takes a scoped piece's cells out of their context, as a line of
their own: the window's edges become free ends.

    S := )xx...x..(
    A := )8: S([5+4]       →  [x..xx]
    Detach(A)              →  <x..xx>

- the result is linear: a piece is already linear in its own terms, and a
  reading only goes one way. `Wind(Detach(A))` makes it a circle
- detaching forgets the context: the parent, and where the piece sat in it. so
  `Detach(A) === A`, the same cells, but they aren't the same spacer: one is
  whole, the other a part. there's no way back, except taking the scope from
  the parent again
- it's what lets a piece be used on its own: joined, or studied as a spacer in
  its own right

#### lift

`Lift(B)` takes one step up a chain of scopes: it gives B as a scope of its
parent's parent, by the rule in "back to the root".

    S := )xx...x..(
    A := )8: S([5+4]       →  [x..xx]       S's cells 5, 6, 7, 0, 1
    B := [5: A][2->4]      →  [.xx]         A's cells 2, 3, 4
    Lift(B)                →  [.xx]         )8: S([7+2]: S's cells 7, 0, 1

- the cells it shows stay the same; what changes is the parent it's mounted
  on. a parent's ends are where a scope has to stop, and the parent above
  always contains the one below, so a lifted piece has at least as much room:
  `Slide(B, 1)` isn't allowed, since it would run past A's end, but
  `Slide(Lift(B), 1)` is `[xx.]`, S's cells 0, 1, 2
- it isn't allowed when the parent is a root or a layer: there's nothing above
  it that a scope can reach
- it forgets the parent it skipped, so there's no way back: many chains lift
  to the same scope

#### press

`Press(K)` takes one step up a cascade: it presses the deepest layer onto the
layer above, so the cascade has one layer fewer. the pressed layer keeps the
upper layer's resolution, and shows the deepest choices on its cells.

    K := |xx...x.x| >> |x.xx| >> |.xx|
    Press(K)               →  |xx...x.x| >> |..xx|
    Press(Press(K))        →  |.....x.x|

- a root with one layer presses to a single spacer, with the root's
  resolution and reading
- it forgets one layer, the way lift forgets one parent: the result doesn't
  show that it was chosen in two steps
- there's no way back: many cascades press to the same one

#### threshold

`Threshold(W, k)` turns a row of amounts back into a spacer: a cell is filled
where its amount is at least k, and the result keeps the row's reading. it's
the bridge from amounts back to states, the way detach is the bridge from a
view. with the stack A, B, C from "collections, stacks and weights":

    W := Weights((A, B, C))     →  |2 2 0 1 0 3 1 0|
    Threshold(W, 1)             →  |xx.x.xx.|       at least one of them
    Threshold(W, 2)             →  |xx...x..|       at least two
    Threshold(W, 3)             →  |.....x..|       all three

for two spacers, `or` and `and` are the two simplest thresholds: `A or B` is
`Threshold(Weights((A, B)), 1)`, and `A and B` is
`Threshold(Weights((A, B)), 2)`.

### named composites

each of these is made of primitives, and its definition says how. `:=` reads
"is defined as", as it does for labels.

#### repeat

`Repeat(L, m)` builds the line that is L written m times in a row, end to end:

    Repeat(L, 3)  :=  Join(L, Join(L, L))
    Repeat(<x..>, 3)        →  <x..x..x..>

the study of strings writes this as a power, and here `L^3` is shorthand for
`Repeat(L, 3)`.

- **linear**: yes
- **fluid**: yes, and the result stays fluid: the copies come out as the same
  cells whichever way it's read, so nothing depends on a borrowed reading.
  `|x..|^3` is `|x..x..x..|`
- **circular**: no. a circle traced round m times is three moves, none of them
  basic, so it's written out when a use wants it: `Wind(Repeat(Unwind(C), m))`
- **scoped**: no. its ends aren't free; detach it first

this builds a new, longer spacer. `~` is different: it says a spacer repeats in
its surroundings, and leaves the spacer as it is.

#### insert

`Insert(H, k, G)` puts a line G into a line H at the joint before H's cell k, k
from 1 to n − 1: H's cells before cell k, then G, then the rest.

    Insert(H, k, G)  :=  Join(Join(Detach(<n: H>[0->k-1]), G), Detach(<n: H>[k->n-1]))
    Insert(<xx..>, 2, <x.>)    →  <xxx...>

at either end, it's simply a join.

#### splice

`Splice(C, k, G)` puts a line G into a circle C at the joint before C's cell k,
k from 1 to n, where k = n is the joint between the last cell and cell 0. C
keeps its cell 0.

    Splice(C, n, G)  :=  Wind(Join(Unwind(C), G))
    Splice(C, k, G)  :=  Rotate(Wind(Join(Unwind(Rotate(C, k)), G)), n - k)
    Splice( )xx..(, 2, <x.>)   →  )xxx...(

at the joint after the last cell, it's unwind, join, wind. at any other joint,
the circle is first rotated so that joint comes round to the end, and rotated
back afterwards.

#### collapse

`Collapse(B)` lifts a scope until its parent is the root, or a layer it can't
look past. the whole chain of scopes in between collapses, and B ends up as one
scope of the top.

    Collapse(B)  :=  Lift(B), again and again, until its parent is a root or a layer

#### flatten

`Flatten(K)` presses a cascade until one spacer is left: its deepest choices,
shown on its root's cells.

    Flatten(K)  :=  Press(K), again and again, until one spacer is left
    Flatten(|xx...x..| >> |x.x|)              →  |x....x..|
    Flatten(|xx...x.x| >> |x.xx| >> |.xx|)    →  |.....x.x|

- the result has the root's resolution and reading
- flattening forgets the layers, the way detach forgets a context: the
  flattened spacer doesn't show that it was chosen in steps (see "layers")
- the order doesn't matter: any run of neighbouring layers, written out, is a
  cascade too, and flattening the top part first, `Flatten(R >> L1) >> L2`,
  ends in the same spacer as pressing from the bottom. flattening a part is
  partial flattening: `Flatten(R >> L1) >> L2`, or `R >> Flatten(L1 >> L2)`
- there's no way back: many cascades flatten to the same spacer

#### but not and xor

these two are written as words between spacers, like the primitives they're
made of (see "combining spacers"):

    A but not B   :=  A and not B
    U := A or B
    I := A and B
    A xor B       :=  U but not I

#### at most and exactly

    AtMost(W, k)   :=  not Threshold(W, k + 1)
    Exactly(W, k)  :=  Threshold(W, k) but not Threshold(W, k + 1)
    Exactly(W, 1)               →  |...x..x.|

#### levels and depths

one threshold loses information, but thresholding at every level loses
nothing. `Levels(W)` thresholds W at every amount from 1 up to its largest, and
puts the results in a cascade: the first level is the root, and each next level
chooses, among the previous level's points, those that reach the next amount.
shown flattened, on the root's cells:

    W := |2 2 0 1 0 3 1 0|
    level 1      x x . x . x x .      Threshold(W, 1): the root
    level 2      x x . . . x . .      Threshold(W, 2), chosen among level 1's points
    level 3      . . . . . x . .      Threshold(W, 3), chosen among level 2's points

`Depths(K)` goes back: for each cell, how many levels of the cascade K it's in.
`Depths(Levels(W))` is W again, and `Levels(Depths(K))` is K again, as long as K
doesn't end in an all-empty layer.

this isn't a coincidence. a row of whole amounts and a cascade are two ways of
writing the same nested structure: counting how deep each cell goes, or listing
the levels. a map shows the same pairing, between heights and the contour lines
around them. what differs is the meaning: a cascade chooses, layer by layer,
while a row of amounts counts.

## going to infinity

every spacer has a finite number of cells. infinity comes in through a
**generator**: a precise rule or process that makes a spacer at each
resolution, n, that it covers. it doesn't have to cover them all: one might
only make spacers with an even number of cells, or only ones that double each
time. the rule has to say exactly which cells are filled; a loose phrase like
"fill the first half" isn't enough, since it leaves cases like 5 cells open.
how the rule is written is up to whoever defines the generator.

how its spacers change as n grows is its **resolution dynamics**, and going to
infinity means following them without end.

what lies at the end isn't a spacer, just as zero isn't: there's no last
resolution, and so no spacer with endless cells. we only ever hold the finite
spacers on the way. zero sits below the smallest spacer and infinity beyond
every one; both are named, and neither is a spacer.

like any origin, a generator lives outside the spacers it makes: a spacer
doesn't show which generator made it.

a generator uses the same brace form as a description, followed by `->` and
what it makes: `Primes{r: 8} -> |..xx.x.x|`. the braces say which spacers are
asked for; the arrow is what makes it a generator. with chance involved, the
arrow gives one possible result: `Coin{r: 8} -> |x..x.xx.|` is one run out of
256 possible. fuller notation for generators will come later: for ones that
only cover a range of resolutions, ones that run on without end, and for how
rules are written (directly cell by cell, step by step from earlier spacers, or
case by case).

as a shorthand, the values can go in round brackets, in a fixed order and
without keys: `Primes(8) -> |..xx.x.x|`. the order comes from the generator's
**signature**, which lists its keys in order, the same way each operation is
introduced with its values in order (`Rotate(S, k)`):

    Primes(r)                    one value: the resolution
    Block(r, d)                  the first d cells filled: first the resolution, then the density
    Block(8, 3)                  means  Block{r: 8, d: 3}
    Block(8, 3) -> |xxx.....|

keys in a signature are mapped to their full names, like any short key. braces
have no order, so in braces every value needs its key. a label directly before
round brackets gives a generator its values; round brackets on their own are a
stack.

generators are described by a few independent **facets**. each can be read on
its own, and any combination can occur, with one exception noted below.

**how spacers relate as n grows**

- **growth/extension**: earlier cells never change; new ones are added. it
  reads like time passing, and fits the linear view, where a line has an end
  to grow from. filling the cells whose number is prime:
  `|..xx.x.x|` → `|..xx.x.x...x|`
- **refinement/subdivision**: each spacer describes the same whole at a finer
  grain, in either view. filling cell j when 2j is less than n:
  `|xx..|` → `|xxx..|` → `|xxx...|` → `|xxxx....|`. what counts as "the same
  whole" depends on the context; refinement is named here as one force in a
  spacer's dynamics, not pinned down
- **both**, or **neither** (each spacer made from scratch)

**how cells are decided**

- **rule/deterministic**: a rule fixes every cell
- **chance/indeterministic**: chance decides, like a coin toss for each new
  cell: `|x..x.xx.|` → `|x..x.xx.x...|` (one possible run)
- **rule and chance**: rules decide which cells chance gets to decide

**how clear the rule is** (only where there's a rule)

- **transparent**: the pattern can be read off and carried forward
- **opaque**: seeing part of the spacer doesn't tell you the rest; you have to
  apply the rule each time, as with the primes. a rule can also be unknown
  altogether: a black box

**how the result looks**

- **patterned**: the arrangement shows a pattern
- **random-like**: it passes for chance, judged against some baseline. the
  primes look random-like once their simplest structure is set aside (beyond
  3, every prime is one more or one less than a multiple of 6)

**where it comes from**

- **natural/found**: nobody designed it; it arises from arithmetic or from
  physics. the primes are found in arithmetic
- **made/designed**: someone built it. a computer's random number generator is
  a rule designed to look random, which is what **pseudo-random** means

how a spacer is decided and how it looks are separate facets. the primes are
decided by a rule but look random-like; choosing at random where to start
reading `)xx..xx..(` is chance, but every result is patterned.

the facets can be read flat, or grouped by where each one lives: in the cells
(how the result looks), in the generator (how spacers relate, how cells are
decided), in what we know (how clear the rule is) and in the origin (where it
comes from). either way, they're the same facets.

## essence and convention

some things about a spacer belong to the spacer itself; others are choices we
make to write it down or talk about it. to tell them apart, imagine two people
drawing the same spacer differently: starting at a different cell, counting
the other way round, using other symbols. what they still agree on is
essence/semantics; what they don't is convention.

- **essence**: the resolution, the density and the sparsity; the distances
  between cells; in the linear view, what sits at the ends. these are found in
  the spacer, not made up
- **convention**: which cell is cell 0, counting from 0, which way the
  numbering runs, the symbols for the two states, and so the written form
  itself. these are chosen, so that everyone starts from the same place

the two views show the difference clearly. a circle has no start, so cell 0 is
pure convention. a line has two ends, so the ends are essence, but which end
comes first is convention.

some things sit in between. which state is filled is written by convention
(■ or `x` here, 1 elsewhere), but the difference it marks, between the things
being spaced and the space between them, isn't one.

## identity

this doc gives identity a **default**: a starting point that any use can
override, tightening or loosening it to fit what it's doing.

**by default, two spacers are the same spacer when they have:**

- the same resolution
- the same reading: both fluid, both circular, or both linear
- the same state in every numbered cell

different encodings never make different spacers: typed or display symbols,
cells or points, or any other symbols that follow the rules. what the encoding
says does matter: the resolution, the reading and the cells. where a spacer
came from isn't part of the default, but a use can add it.

so by default, `|8: 0,1,5|` is the same spacer as `|xx...x..|`. but `)x.x..(`
and `)x..x.(` are different (one is the other rotated), and so are `|x..|` and
`<x..>` (one is fluid, the other committed to linear).

a scoped piece and a whole linear spacer with the same cells, like `[x..]` and
`<x..>`, aren't the same spacer by default: one is a piece of something larger,
the other is the whole thing. they are alike, though: the same cells, behaving
the same way within their ends, so a use that only cares about the cells can
set the context aside and treat them as the same, written `[x..] === <x..>`
(see below). likewise, two cascades that flatten to the same spacer are alike,
giving the same cells, but whether they're the same cascade, layer by layer, is
a higher-level question, left to the uses that need it.

comparing cell by cell includes the conventions, cell 0 and the direction. that
doesn't make them essence (see "essence and convention"): the default is simply
the strictest comparison, so it never merges two spacers that a use might need
to keep apart.

a fluid spacer **fits** a circular or linear spacer with the same resolution and
cells: it could become it, just by committing to a reading. fitting isn't
identity, and it runs one way only, from the open reading to a committed one:
`|x..|` fits both `)x..(` and `<x..>`, but those two are still different
spacers. between them, unwind and wind convert either way, losing nothing.

that gives five ways of comparing:

| comparison                 | means                                                     | typed | display |
| -------------------------- | --------------------------------------------------------- | ----- | ------- |
| **identical**              | the same resolution, reading and cells (the default)      | `=`   | `=`     |
| **fits**                   | a commitment away: one fluid, the other committed         | `~=`  | `≃`     |
| **converts**               | a conversion away: one circle, one line, the same cells   | `<->` | `⇄`     |
| **the same up to reading** | any of the three above                                    | `==`  | `≈`     |
| **matches**                | the same number of cells and the same value in every numbered cell, whatever the reading, context or origin | `===` | `≍` |

    |x..| ~= )x..(        fits
    )x..( <-> <x..>       converts
    |x..| == <x..>        the same up to reading: here, because it fits
    )x..( == <x..>        the same up to reading: here, because it converts
    [x..] === <x..>       matches: a piece and a whole line, with the same cells
    [x..] === )x..(       matches: a piece and a whole circle; only the cells count

the more `=` signs, the more is set aside: `=` sets aside nothing, `==` the
reading, and `===` the reading and the context. (this runs opposite to
javascript, where `===` is the strictest.)

`==` is whichever of `=`, `~=` and `<->` applies. `=`, `==` and `===` chain: if
A is the same as B, and B as C, then A is the same as C. `~=` and `<->` don't:
two fittings from the same fluid spacer give a conversion, and two conversions
come back to the same reading, which is identity. `<->` needs spaces around it,
so it's never read as edges.

`===` compares values, so it works for rows of weights too, where the values
are amounts rather than states. the numbering still counts: `)x..( === ).x.(`
is false, since setting the start aside is a separate loosening.

**overriding the default.** the default asks whether the entire thing is the
same. being alike, matching in some respects and behaving similarly, is a
looser question, and the overrides below are ways of asking it. a use can
loosen the default by setting something aside, or tighten it by adding
something. the usual loosenings have exact forms through
the operations:

- **up to start**: the same when one is a rotation of the other
- **up to direction**: the same when one is a reversal of the other
- **up to reading**: `==`, the same cells whatever the reading

tightening adds what the default leaves out, like where a spacer came from, or
its frame in a stack. a use that overrides the default should say what it
compares and what it sets aside.

## a note on "cell"

"cell" has established meanings elsewhere, and some come close to this one:

- **cellular automata**: the simplest kind has cells along one dimension, each
  in one of two states, the same picture as a spacer. but there the cells are
  updated by a rule, step after step. a spacer has no rule and no steps; it's
  just the arrangement
- **topology**: cells are the pieces a space is glued together from, in any
  number of dimensions. close in spirit, but none of that theory is assumed
  here
- **everyday uses** (spreadsheets, computer memory, biology): a cell holds or
  does something

here a cell is only this: one piece/slot of a spacer's space, with a position
and one of two states.
