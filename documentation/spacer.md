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

the two views are connected: cut the circle between two neighbouring cells and
it becomes a line; join a line's two ends and it becomes a circle again.

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
- **neutral**: stepping works between the ends; what happens past them is
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

a **seam** is a place where neighbouring cells are in different states. the
seams cut a spacer into **bands**: stretches of cells in the same state, each
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
    |xx...x..|    neutral       plain walls: just the spacer, no view chosen

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
edges do in the typed form. the figures before this section use the neutral
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
label stands for its cells: `)S(` is S read circularly. like the point form,
it can carry its resolution: `)6: S(`.

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
- **neutral**: in `|6: S|[…]`, what lies past the ends is undecided, so a
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
a piece is one unbroken stretch of the spacer the chain was first taken from,
its **root**. if that spacer is itself a layer (see "layers"), the chain stops
there, in the layer's numbering. to find where a piece sits in its root, go up
one level at a time: add the parent's start to the piece's start. if the parent
walked −, subtract instead, and flip the direction. you can stop at any level
on the way up. a start can come out below 0, which can only happen in a
circular root: at resolution 8, −2 and 6 name the same cell, but −2 keeps how
it was reached. either can be written.

    B starts at A's cell 2, and A starts at S's cell 5:   5 + 2 = 7
    B in S:   )8: S([7+2]  =  )8: S([7->9]          →  [.xx]

    C := )8: S(-[1+4]      →  [xx..x]      S's cells 1, 0, 7, 6, 5
    D := [5: C][1->3]      →  [x..]        C's cells 1, 2, 3
    D in S: C walked −, so 1 − 1 = 0, going −:   )8: S(-[0+2]   →  [x..]

the chain records how a piece was made; the root form records where it ends
up. they always agree.

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
  steps; like any origin, the cascade lives outside the cells

layers aren't scopes. a scope takes a stretch of neighbouring cells; a layer
chooses among points, skipping the cells in between. both nest and both chain,
and they can be mixed: a scope of a layer is a stretch of consecutive points.

a scope of a layer stays in the layer's context: going back up through scopes
stops at the layer. placing it on the cascade's root means following the layers
up, which is flattening, not scoping, and a stretch of consecutive points in a
layer can be scattered across the root: in `|xx...x..| >> |x.x|`, the layer's
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

## sets, stacks and weights

a **set** groups spacers together, each at most once, in no order: `{A, B, C}`.
a description is a set too: `{r: 8, d: 3}` is every spacer that fits.

a **stack** lines spacers of the same resolution up cell by cell, in the order
they're put on: `(A, B, C)`. a stack can hold the same spacer more than once,
and each copy counts.

    cell      0 1 2 3 4 5 6 7
    A         ■ ■ ◇ ◇ ◇ ■ ◇ ◇
    B         ■ ◇ ◇ ■ ◇ ■ ◇ ◇
    C         ◇ ■ ◇ ◇ ◇ ■ ■ ◇
              ---------------
    weights   2 2 0 1 0 3 1 0

- the **cell weights** count the filled cells in each column. they don't depend
  on the order, so a set of spacers with the same resolution gives the same
  weights as a stack: `{A, B, C}` and `(A, B, C)` agree here
- the weights live in a space of their own: the same cells and reading as the
  stack, but each cell holds an amount instead of one of two states. so a row
  of weights isn't a spacer
- the weights add up to the members' densities together (9 here), and no
  weight is more than the number of members
- a member's **score** is the weights added up at its points: how much it
  agrees with the whole, itself included. A scores 2 + 2 + 3 = 7; B and C
  score 6 each
- a stack's **build-up** is its running weights as each member goes on.
  unlike the final weights, it depends on the order:

      after A    1 1 0 0 0 1 0 0
      after B    2 1 0 1 0 2 0 0
      after C    2 2 0 1 0 3 1 0

weights are a derived result, and for most stacks they're just that: a count.
but when the spacers going in are patterned, the weights can carry structure of
their own, and become objects worth studying in their own right: a space of
amounts, where the amount works almost like an extra dimension. that world is
noted here, not explored.

weights are written as a row of numbers, which are amounts, not states (rule
4): `|2 2 0 1 0 3 1 0|`.

("weight" has another meaning in coding theory, where a string's weight is its
number of 1s; here, that's density. cell weights are always per cell, across a
set or stack.)

stacking lines cells up by their numbers, so it takes each spacer exactly as
written: `)x.x..(` and `)x..x.(` add to different columns, although read
circularly one is the other started from a different cell. combining spacers
is one of the places where identity gets decided.

the reading carries through: circular spacers give circular weights, and
linear ones give linear weights. a stack of neutral spacers leaves the reading
open, so its weights can be read however the use needs. writing the members
circular or linear says the reading is part of the structure; writing them
neutral says it's a perspective for later. for now, a stack can't mix readings.

## combining spacers

two spacers with the same resolution can be combined cell by cell into a new
spacer. like stacking, this lines their cells up by number, as written, and the
same reading rules apply: both circular, both linear or both neutral, and the
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
`A ∪ B`, `A △ B`, `A ∖ B`.

they're also thresholds on weights: stack A and B, and `A and B` is where the
weight is 2, `A or B` where it's at least 1, and `A xor B` where it's exactly 1.
with more spacers in a stack, any threshold works the same way, such as "at
least half of them".

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
without keys: `Primes(8) -> |..xx.x.x|`. the order has to be stated for each
generator, the way a short key is mapped: for `Primes`, the one value is the
resolution. braces have no order, so in braces every value needs its key. a
label directly before round brackets gives a generator its values; round
brackets on their own are a stack.

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

when two spacers count as the same (identity), or as alike (equivalence), is
left open here on purpose. the right answer depends on the use, so it's best
decided as late as possible, by whoever is working with spacers, in the way
that fits what they're doing.

one thing holds whatever is decided: how a spacer is written never makes it a
different spacer. typed or display, cells or points, or any other symbols that
follow the rules all write the same thing.

when identity or equivalence does get decided, it helps to say what's being
compared and what's being set aside: the resolution, the density, the
arrangement, the reading, the start and direction, or where a spacer came
from.

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
