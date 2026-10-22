# prior work

existing research that overlaps with the ideas in
[fur-tea-laser/math](https://github.com/fur-tea-laser/math), so you know what's
already been done, what to read, and what might still be new.

experiments and findings: [SESSION_2026-10-19.md](SESSION_2026-10-19.md), [SESSION_2026-10-20.md](SESSION_2026-10-20.md).

## contents

- [positioning](#positioning)
- [how sure each claim is](#how-sure-each-claim-is)
- [spacer](#spacer)
  - [timeline](#timeline)
  - [concept map](#concept-map)
  - [possibly new](#possibly-new)
- [prime](#prime)
  - [todo: check patterns against fake number lines](#todo-check-patterns-against-fake-number-lines)
- [loop](#loop)
- [broader fields](#broader-fields)
  - [continuous perspective](#continuous-perspective)
  - [from discrete to continuous](#from-discrete-to-continuous)
- [to explore: values with more dimensions](#to-explore-values-with-more-dimensions)
- [reading list](#reading-list)
- [search terms](#search-terms)

## positioning

### why new names for established ideas

much of what's here exists already under other names. a spacer is a binary
necklace. a euclid spacer is a euclidean rhythm (Toussaint) and a maximally
even set (Clough & Douthett). a recursive spacer is nth-order maximal evenness
(Douthett). a loop layer is Hügelschäffer's egg construction. prime containers
and tribe widths are twin-prime midpoints and twin-prime gaps.

the renaming isn't a claim to those ideas, and this document credits each one.
the problem it solves is that the same structures were studied separately, by
different communities, each with its own vocabulary:

| community                | calls it                          | focus                      |
| ------------------------ | --------------------------------- | -------------------------- |
| rhythm / computational geometry | euclidean rhythm, onset pattern | beats in a cycle           |
| music theory             | maximally even set, scale, chord  | pitches in an octave       |
| combinatorics on words   | Christoffel / Sturmian word, necklace | strings                |
| computer graphics        | Bresenham line                    | pixels                     |
| number theory            | prime constellation, twin gap     | primes on the number line  |

each vocabulary is tied to its domain: a "scale" assumes pitch, a "rhythm"
assumes time, a "constellation" assumes primes. the spacer vocabulary assumes
none of them.

### what a spacer is

a spacer is deliberately plain: a binary cycle (`[resolution, points]`) plus
derived measurements (density, intervals, weights) and defined operations
(phase, orientation, layering, grouping). anything that can be written as a
binary cycle can use all of it.

that does two things at once:

- **decouples** each idea from the domain it was found in. evenness isn't
  about music, and layering isn't about chords
- **centralizes** fragmented work in one representation with one set of tools,
  so a result or operation from one field applies to the others

### the payoff: primes as rhythms

the clearest example is the prime tribe. the primes between two consecutive
twin-prime pairs, measured from the lower pair, become a spacer:

    tribe 7: containers 72 → 102, primes 73, 79, 83, 89, 97, 101
    tribeSpacer(7) = [30, [0, 6, 10, 16, 24, 28]]

once a tribe is a spacer, everything built for rhythms applies to it with no
new code: intervals, relative points, slot weights across a set of tribes,
comparison against the euclid spacer of the same resolution and density (how
"even" are the primes?), and phase and orientation. number theory has studied
the width of tribes (twin-prime gaps) and how many primes they hold (OEIS
A027833), but nothing found treats the full pattern inside as a rhythm.

### what this is and isn't

- **is**: an abstraction and a toolkit, a common lens for patterns that were
  studied separately, and a way to ask cross-domain questions
- **isn't**: a claim of new theorems. the abstraction itself is closer to
  software design than to mathematics. anything new will come from what the
  lens reveals
- **caveat for primes**: many apparent prime patterns come from divisibility
  by small primes (every prime > 3 is 6k ± 1, for example). a pattern found
  through spacers only means something if it survives comparison with a
  baseline that has the same small-prime structure

## how sure each claim is

- **verified**: confirmed from a source found during research (2026-10-19)
- **from memory**: recalled but not confirmed against a source; check before
  citing
- **unconfirmed**: something I looked for but couldn't find
- **derived**: follows from the definitions plus standard facts, not from a
  paper
- **computed**: checked by computation only

## spacer

### starting point

**Toussaint, "The Euclidean Algorithm Generates Traditional Musical Rhythms"**
(2005, Bridges conference, Banff)
— [pdf](https://cgm.cs.mcgill.ca/~godfried/publications/banff.pdf)

connects Bjorklund's timing algorithm to rhythms found across world music. it
covers single-layer euclidean rhythms. I couldn't extract the pdf's text, so
whether it cites Clough & Douthett is _unconfirmed_.

### timeline

| year   | work                                                                                                       | relevance                                                                                                                                                                                 | status      |
| ------ | ---------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- |
| 1870s  | Christoffel words                                                                                          | the same binary patterns as euclid spacers, studied in combinatorics on words (also called mechanical or Sturmian words)                                                                 | from memory |
| 1965   | Bresenham's line algorithm                                                                                 | drawing a digital line produces the same even distributions                                                                                                                              | from memory |
| 1991   | Clough & Douthett, "Maximally Even Sets," _Journal of Music Theory_ 35, pp. 93–173 (doi:10.2307/843811)   | defines maximally even sets with ⌊(ck + m)/d⌋, which gives the same patterns as `basicEuclidSpacer`; `m` shifts the pattern like phase does. the generating "J-function" appears near the end of the paper, though it isn't named there yet | verified    |
| ~1991+ | "second-order maximal evenness" (credited to Douthett)                                                     | a maximally even subset of a maximally even set, i.e. a **two-layer aligned recursive spacer**. the example is a triad as 3-in-7-in-12, i.e. `[12, [7, o], [3, o]]`. extends to nth order | verified    |
| 1992   | Lerdahl, "Cognitive Constraints on Compositional Systems," _Contemporary Music Review_ 6(2)               | a "reductional format" for pitch space that resembles nested evenness                                                                                                                     | verified    |
| 2003   | Bjorklund, timing algorithm for spallation neutron source (SNS) accelerators                              | the algorithm `coreEuclidMap` closely resembles                                                                                                                                          | from memory |
| 2005   | Toussaint, Banff paper                                                                                     | the starting point (see above)                                                                                                                                                           | verified    |
| 2007   | Douthett & Krantz, "Maximally even sets and configurations," _J. Comb. Optim._                             | generalizes maximal evenness across math, physics and music                                                                                                                              | verified    |
| 2007   | Amiot, "David Lewin and Maximally Even Sets," _Journal of Mathematics and Music_ 1(3)                      | discrete fourier transform view: a maximally even set of size k has a maximal k-th fourier coefficient                                                                                    | verified    |
| 2007   | Demaine et al., "The Distance Geometry of Music"                                                          | euclidean rhythms maximize the sum of pairwise distances, and nearly all of them are "deep"                                                                                              | verified    |
| 2008   | Douthett, "Filtered Point-Symmetry and Dynamical Voice-Leading," in _Music Theory and Mathematics_ (U. Rochester Press), pp. 72–106 | **the closest match.** models iterated maximally even sets (3→7→12, 3→8→12) as concentric circles with filters; rotating the inner circles moves between members, much like a spacer group | verified    |
| 2009   | Gómez-Martín, Taslakian & Toussaint, "Interlocking and Euclidean rhythms," _Journal of Mathematics and Music_ | takes subsets of a euclidean rhythm's onsets ("alternation") and asks when the result is still euclidean. the nearest rhythm-side work to recursion                                     | from memory |
| 2016   | Amiot, _Music Through Fourier Space_ (Springer)                                                            | book-length treatment of the fourier approach                                                                                                                                            | verified    |
| 2022   | "Jack Douthett and mathematical music theory," _Journal of Mathematics and Music_                          | retrospective: describes filtered point-symmetry as a visualization of nth-order maximal evenness and the J-functions behind it. **best overview to start with**                        | verified    |

### concept map

| your term                     | existing term(s)                                                                     |
| ----------------------------- | ------------------------------------------------------------------------------------ |
| spacer                        | binary necklace / rhythm / pitch-class set on a circle                               |
| euclid spacer                 | euclidean rhythm; maximally even set; Christoffel word (up to rotation)              |
| core euclid spacer            | the primitive (gcd-reduced) pattern; a primitive necklace                            |
| spacer phase                  | rotation / transposition (the `m` in ⌊(ck + m)/d⌋)                                   |
| spacer orientation            | rotation onto a point; a "mode" in scale terms                                      |
| recursive euclid spacer       | second-order / nth-order maximal evenness; iterated maximally even sets              |
| spacer group                  | roughly the rotating inner circles of filtered point-symmetry                        |
| spacer interval               | inter-onset interval (rhythm); step pattern (scales)                                 |
| relative spacer point         | onset as a fraction of the cycle / point on the unit circle                         |

### possibly new

nothing found for these, but the search was not exhaustive:

- framing nested evenness as **rhythm**; the music-theory work is almost all
  about pitch (scales, chords, voice leading)
- **orientation measured in points** as a separate thing from phase, with an
  independent orientation and phase at **every layer** (phased recursive
  spacers)
- **lineage**: every group a spacer belongs to, organized by altitude
- **slot weights, point weights and spacer weight** as a way to score a spacer
  against its group
- the **prime tribe → spacer** connection

## prime

### concept map

| your term                | existing term / fact                                                                                                                                                                                                  | status   |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| prime container          | the average of a twin prime pair, OEIS [A014574](https://oeis.org/A014574) (4, 6, 12, 18, 30, 42, 60, 72, …). every term after 4 is a multiple of 6. OEIS also lists it as "n such that n² − 1 is a semiprime"  | verified |
| tribe width              | the gap between consecutive twin prime pairs. record widths are OEIS [A113274](https://oeis.org/A113274) "record gaps between twin primes" (2, 6, 12, 18, 30, 36, 72, 150, …). every width after 2 is a multiple of 6 | verified |
| prime tribe              | the primes between consecutive twin pairs: the upper twin of one pair, then **isolated primes** (OEIS [A007510](https://oeis.org/A007510)), then the lower twin of the next pair                                   | verified |
| tribe size               | **OEIS [A027833](https://oeis.org/A027833)** (1, 2, 2, 3, 3, 4, 3, 6, 2, 5, …), listed as "distances between successive 2's in the differences between consecutive primes". a 2012 comment (Ordowski) notes it is the number of primes between consecutive A014574 terms, i.e. tribe size. comments include the conjecture a(n) < (log container)², and that every positive integer below 312 appears (Greathouse, 2016). it equals 2 + the isolated primes between two twin pairs, except in the first tribe, where 5 belongs to both twin pairs | verified |
| infinitely many tribes   | exactly the **twin prime conjecture** (open)                                                                                                                                                                          | derived  |
| tribe spacer offsets     | every prime > 3 is 6k±1, so tribe spacer points always fall on offsets ≡ 0 or 4 (mod 6)                                                                                                                               | computed |
| tribe of width 6         | two twin pairs 6 apart, i.e. a **prime quadruplet** (p, p+2, p+6, p+8)                                                                                                                                                | derived  |
| tribe spacer (in general) | the pattern of primes between two twin pairs, i.e. a **prime constellation** with no twin pair inside it. the Hardy–Littlewood k-tuple conjecture predicts how often each admissible pattern occurs              | derived  |
| `numberGreaterThanPrime` | the bound pₙ < n(ln n + ln ln n) for n ≥ 6, which follows from Rosser & Schoenfeld, "Approximate formulas for some functions of prime numbers," _Illinois J. Math._ 6 (1962), via a sharper bound for n ≥ 20 | verified (secondary sources) |

### computed data

computed 2026-10-19 with an awk sieve, for tribes with containers below
2,000,000:

- **14,870 tribes.** the README examples check out: `tribeSpacer(2) = [6, [0, 4]]`,
  `tribeSpacer(3) = [12, [0, 4, 10]]`, `tribeSpacer(4) = [12, [0, 6, 10]]`
- **zero exceptions** to the offsets ≡ 0 or 4 (mod 6) rule
- **10,860 distinct tribe spacers.** most patterns occur once, but small ones
  repeat (`[6, [0, 4]]` is every prime quadruplet)
- **tribe sizes** fall off steadily: 2 members: 1,512; 3: 1,459; 4: 1,335;
  5: 1,109; … up to one tribe with 103 members
- **widest tribe:** width 1,452, containers 850,350 → 851,802. this matches
  A113274's record gap of 1,452, which cross-checks the computation
- **most common widths** below 60: 30 (927), 42 (846), 12 (693), 60 (596),
  18 (511). widths divisible by 5 or 7 show up more than their neighbours.
  this looks like the Hardy–Littlewood "singular series" effect, where gaps
  divisible by more small primes are favoured (for ordinary prime gaps it's
  known as **jumping champions**, Odlyzko, Rubinstein & Wolf 1999, from
  memory). not checked against a source for twin gaps

### twin prime literature

| year      | work                                                                                                                                                                           | relevance                                                                                                          | status      |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------ | ----------- |
| 1919      | Brun's theorem                                                                                                                                                                 | the sum of reciprocals of twin primes converges. OEIS: by Brun, "almost all primes" are isolated, so tribes grow   | verified (via OEIS) |
| 1922–23   | Hardy & Littlewood, prime k-tuple conjecture                                                                                                                                  | predicts how often each prime pattern occurs; the twin prime constant ≈ 1.32032 comes from here                   | verified (via OEIS) |
| 2001      | Marek Wolf, ["Some Remarks on the Distribution of twin Primes"](https://arxiv.org/abs/math/0105211)                                                                          | **closest match for tribe widths**: computes the gaps between consecutive twin pairs up to 2⁴⁴ and fits a heuristic formula | verified    |
| 2013      | Kourbatov, ["Maximal Gaps Between Prime k-Tuples: A Statistical Approach"](https://cs.uwaterloo.ca/journals/JIS/VOL16/Kourbatov/kourbatov3.html), _J. Integer Sequences_ 16 | record gaps between twin pairs (= record tribe widths), Gumbel-distributed around a trend curve                    | verified    |
| 2013      | Kourbatov, ["Tables of record gaps between prime constellations"](https://arxiv.org/abs/1309.4053)                                                                             | record gap tables                                                                                                  | verified    |
| 2013–14   | Zhang; Maynard; Polymath8                                                                                                                                                      | bounded gaps between primes (currently ≤ 246). twin primes would be a gap of 2                                    | from memory |
| 2019      | Kourbatov & Wolf, ["Predicting maximal gaps in sets of primes"](https://arxiv.org/abs/1901.03785)                                                                             | general framework for gaps in prime subsets                                                                        | verified    |
| ongoing   | Tomás Oliveira e Silva, ["Gaps between twin primes"](https://sweet.ua.pt/tos/twin_gaps.html)                                                                                  | computational record tables                                                                                        | verified    |

### todo: check patterns against fake number lines

**reminder:** before calling a tribe-spacer pattern "emergent", check whether
fake primes produce it too.

**the idea.** make a fake number line by going through the numbers and flipping
a weighted coin for each one: heads = "prime". then run the exact same pipeline
on the fake ✓ marks (twin pairs → containers → tribes → tribe spacers →
weights, etc.) and compare with the real primes.

- a coin knows nothing about primeness, so a pattern the fake also shows comes
  from the rules the coin was given (or from the lens itself)
- a pattern only the real primes show is the interesting kind

**the rules.** each fake version only changes which numbers get a coin flip.
the coin's weight is set so the fake has about as many "primes" as the real
line (density ≈ 1/ln n):

| version | which numbers get a coin flip                    | copies which prime rule      |
| ------- | ------------------------------------------------ | ---------------------------- |
| fake A  | only 6k ± 1                                      | not divisible by 2 or 3      |
| fake B  | only 6k ± 1 that also aren't multiples of 5 or 7 | not divisible by 2, 3, 5, 7  |
| next    | also skip multiples of 11, 13, …                 | more small primes            |

**first result** (computed 2026-10-19, up to 2,000,000): how often each tribe
width occurs

| width | real | fake A | fake B |
| ----- | ---- | ------ | ------ |
| 6     | 2.0% | 5.1%   | 2.2%   |
| 12    | 4.7% | 5.1%   | 6.2%   |
| 24    | 2.3% | 4.1%   | 1.9%   |
| 30    | 6.2% | 4.2%   | 7.6%   |
| 36    | 1.8% | 3.7%   | 1.8%   |
| 42    | 5.7% | 3.9%   | 5.6%   |
| 60    | 4.0% | 3.3%   | 4.5%   |

fake A has no peaks, fake B reproduces them. so the 30/42 peaks are explained
by divisibility by 5 and 7: they come from the sieve, not from anything
deeper.

**still to figure out**

- how the fake rules and the coin's weight are chosen, in detail (see Cramér
  and Granville below)
- try a tiny fake line by hand (up to ~100) to get a feel for it
- add 11, 13, … to the fake and see whether the remaining differences (e.g.
  width 12: 4.7% vs 6.2%) shrink
- run the spacer tools (slot weights, evenness vs the euclid spacer) on fake
  tribes as well as real ones

**the sketch** (the awk script was in a temporary folder and won't be kept):

    for each n:
      if n passes the rules (e.g. gcd(n, 210) = 1):
        mark n "prime" with probability (210 / 48) / ln(n)
    # 48 = how many of every 210 numbers pass the rule,
    # so the extra factor keeps the overall density at ~1/ln n
    then: twin pairs → containers → tribes, as for real primes

**prior work** (from memory, check before citing): random primes with matching
density are **Cramér's model** (Harald Cramér, 1936). making the random model
respect small primes is **Granville's refinement** (Andrew Granville, 1995,
"Harald Cramér and the distribution of prime numbers").

### possibly new

- treating the interval between twin pairs as a **unit** (a tribe) with its own
  index, size and internal pattern. the literature studies the **width**
  (twin gaps), and OEIS has the **size** (A027833), but nothing found on the
  full **pattern** inside as an object
- **tribe spacers**: encoding each tribe's internal pattern as a spacer, so the
  spacer tools (intervals, weights, groups) can be applied to primes

"derived" means it follows from the code's definitions plus standard facts,
not from a paper. "computed" means checked by computation only.

## loop

### starting points

- [egg curves](https://www.mathematische-basteleien.de/eggcurves.htm)
  (Mathematische Basteleien, Jürgen Köller), a catalog of egg constructions
- [circles sines and signals](https://jackschaedler.github.io/circles-sines-signals/)
  (Jack Schaedler), an interactive DSP primer: rotating phasors projecting to
  sine and cosine

### one layer is a Hügelschäffer egg

what one layer of `loopPoint` does:

1. place a circle of radius `r` inside the unit circle, with its centre `c` at
   distance `d·(1 − r)` in direction `phase`
2. take the point `s` at angle θ on that small circle
3. extend the ray from `c` through `s` until it hits the unit circle, at `b`
4. output `(b.x, s.y)`, i.e. x from the big circle and y from the small one

this is **Hügelschäffer's egg construction** (Fritz Hügelschäffer,
_Elemente der Mathematik_ 3, 1948), with the big circle's radius fixed at 1.
Hügelschäffer adapted **de La Hire's ellipse construction** (two concentric
circles, x from the big one and y from the small one) to circles whose centres
are offset.

**checked by derivation** (2026-10-19), not just by comparing descriptions.
with the big circle radius `a`, small radius `b` and small centre at `(m, 0)`,
the construction gives

    y² = b²(a² − x²) / (a² − 2mx + m²)

which, with `m = −d`, is the equation on the egg-curves page,
`x²/a² + y²/b²·[1 + (2dx + d²)/a²] = 1`.

how the layer parameters map:

| loop layer field    | geometry                                                                                                                                     |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| `relativeSubLoopRadius` | `b/a`, the egg's width relative to its length                                                                                           |
| `relativeSubLoopDepth`  | the centre offset `d`, scaled so the small circle stays inside: 0 = ellipse, toward 1 = more pointed egg                               |
| `subPhase`          | the direction of the offset. off the x-axis, the egg loses its axis of symmetry (de.wikipedia "Eilinie" describes this for de La Hire)       |
| `subOrientation`    | rotates the layer's output about the global origin                                                                                            |
| `loopRotation`      | rotates the output about the small circle's centre                                                                                            |

### stacking layers (possibly new)

each outer layer runs the same construction, but with the inner layer's
**curve** where the small circle would be. the result is an egg built from an
egg, and so on. the closest work found:

| work                                                                                                                                                      | relevance                                                                                                         | status                           |
| --------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- | -------------------------------- |
| Obradović, Malešević, Petrović & Djukanović (2013, _Scientific Bulletin of the "Politehnica" University of Timișoara_ 58(72), ICEGD 2013), ["Generating Curves of Higher Order Using the Generalisation of Hügelschäffer's Egg Curve Construction"](https://www.researchgate.net/publication/332530776_Generating_Curves_of_Higher_Order_Using_the_Generalisation_of_Hugelschaffer%27s_Egg_Curve_Construction) | generalizes the construction to get higher-order curves. **read this first**: whether it swaps the circles for other curves (as stacking does) is unconfirmed | found, contents unconfirmed      |
| ["Egg curves and generalisation of Hügelschäffer's construction"](https://www.researchgate.net/publication/332530212_Egg_curves_and_generalisation_Hugelschaffer's_construction_Master_thesis_in_Serbian_Jajaste_krive_i_generalizacija_Hugelseferove_konstrukcije) (master's thesis, Serbian) | same line of work                                                                                                 | found, contents unconfirmed      |

| ["What Is an Oval, Officially and Overall? Old and New Mathematical Descriptions"](https://www.mdpi.com/2079-3197/14/5/101) (_Computation_ 14(5), 2026) | recent survey of oval constructions. if nested egg constructions are published anywhere, a survey like this should mention them | found, couldn't open (403)       |

nothing found that nests the construction recursively, or that treats it as a
signal generator. this is **not** a novelty claim: about six searches, and
the three most relevant sources couldn't be opened.

### egg research (ongoing)

| year | work                                                                                                                                                                                        | relevance                                                      | status   |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------- | -------- |
| 1948 | Hügelschäffer, _Elemente der Mathematik_ 3, pp. 67–68                                                                                                                                       | the construction                                               | verified |
| 2010 | Petrović & Obradović, the curve's equation (a cubic)                                                                                                                                        | algebra of the curve                                           | verified |
| 2013 | Petrović, Obradović & Mijailović, ["Suitability analysis of Hügelschäffer's egg curve application in architectural structures' geometry"](https://www.researchgate.net/publication/258113014_Suitability_analysis_of_Hugelschaffer%27s_egg_curve_application_in_architectural_structures%27_geometry) | architecture                                                   | verified |
| —    | Petrović & Obradović, ["The Complement of the Hügelschäffer's construction of the Egg Curve"](https://www.researchgate.net/publication/273139632_The_Complement_of_the_Hugelschaffer's_construction_of_the_Egg_Curve) | the construction's other branches                              | verified |
| 2021 | Narushin, Romanov & Griffin, "Egg and math: introducing a universal formula for egg shape," _Annals of the NY Academy of Sciences_ (doi:10.1111/nyas.14680)                                | fitting real bird eggs; earlier work uses Hügelschäffer's model | verified |
| 2023 | Petrović & Malešević, ["Hügelschäffer egg curve and surface"](https://arxiv.org/abs/2205.14448), _Applicable Analysis and Discrete Mathematics_ 17(1)                                     | area (elliptic integrals), the 3D egg surface, volume          | verified |
| 2024 | Petrović & Malešević, ["The area of Hügelschäffer curves via Taylor series"](https://www.pmf.ni.ac.rs/filomat-content/2024/38-23/38-23-5-24025.pdf), _Filomat_ 38                        | parametrization, area                                          | verified |

### loops as signals

`loopCosine` and `loopSine` read x and y off the curve as θ goes around, the
same way a phasor gives sine and cosine. `loopPendulum` is the angle between
the output point and the base point, which works as a phase-modulation signal.

| work                                                                                                                                                  | relevance                                                                                                                                         | status      |
| ----------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- |
| phase distortion synthesis (Casio CZ series, 1984)                                                                                                     | warping how fast a sine's phase advances. a loop does this geometrically                                                                         | from memory |
| Kleimola, Lazzarini, Timoney & Välimäki, ["Vector Phaseshaping Synthesis"](https://mural.maynoothuniversity.ie/4096/1/vps_dafx11.pdf) (DAFx 2011)      | 2D phase distortion with breakpoint vectors                                                                                                       | verified    |
| polygonal synthesis: Chapman, then Hohnerlein et al. (DAFx, ~2016)                                                                                     | **closest match**: a phasor traces a shape (a polygon) to make a waveform, with a continuous order parameter for morphing                          | verified (year from memory) |
| Argentieri & Scagliola, ["Arbitrary Polygon Oscillator"](https://arxiv.org/abs/2608.24726) (DAFx26, Aug 2026)                                          | **ongoing**: any closed shape as a waveform source, morphing, 3D. a loop would be another kind of source shape                                    | verified    |
| wave terrain synthesis (1970s–80s)                                                                                                                     | reading a signal off a 2D surface along a path                                                                                                    | from memory |
| oscilloscope music / XY laser art (e.g. Jerobeam Fenderson)                                                                                            | stereo audio drawn as XY curves on a scope or laser. a loop's (cos, sin) pair is directly an XY signal                                            | from memory |

### related curve families

- **de La Hire's construction**: the concentric case (an ellipse)
- **epicycles / spirographs / hypotrochoids**: nested circles too, but they
  **add** rotations, where a loop **projects** between circles
- **superellipses** (Lamé, Piet Hein) and the **superformula** (Gielis, 2003,
  from memory): other single-formula shape families
- **Cassini and Cartesian ovals, Granville's egg**: other egg constructions on
  the egg-curves page

## broader fields

there's no single field of "binary spacings", but several fields study the
same object (a pattern of 1s and 0s on a line or cycle) from different angles.
euclid spacers and prime tribes both fit in this cluster.

everything here is _from memory_ unless marked **verified**.

### three ways to read a spacer

| read it as…               | example     | field                                                            |
| ------------------------- | ----------- | ---------------------------------------------------------------- |
| a **string** of 0s and 1s | `11010`     | combinatorics on words, symbolic dynamics                        |
| **points** on a line      | dots at 0, 1, 3 | point patterns, spacing statistics, aperiodic order, discrepancy |
| a **set of integers**     | {0, 1, 3}   | additive combinatorics                                           |

`[resolution, points]` already uses all three.

### the fields

**combinatorics on words**: strings over a small alphabet. the closest thing
to a general theory of binary sequences.

- euclid spacers are **Sturmian / Christoffel words**, the most **balanced**
  binary words ("balanced" is the formal version of evenness)
- **factors** are the substrings that appear in a sequence. a tribe spacer is a
  factor of the prime sequence, cut out between twin pairs
- reference: Lothaire, _Combinatorics on Words_ (1983), and its sequels
  _Algebraic Combinatorics on Words_ and _Applied Combinatorics on Words_

**symbolic dynamics**: infinite sequences under the shift map.

- spacer phase is the shift
- measures complexity: how many distinct patterns of length n appear, and
  entropy
- reference: Lind & Marcus, _An Introduction to Symbolic Dynamics and Coding_
  (1995)
- **closest to the prime work** (verified): since Sarnak (2010), the binary
  sequence of **squarefree numbers**, and more generally sieved **B-free
  numbers**, has been studied as a dynamical system. the squarefree system's
  entropy is 6/π². the minimal part of a B-free system is a **Toeplitz
  sequence**, built by filling gaps level by level, a cousin of recursive
  spacers
  - El Abdalaoui, Lemańczyk & de la Rue, ["A dynamical point of view on the set of B-free integers"](https://arxiv.org/abs/1311.3752) (2013)
  - Bartnicka, Kasjan, Kułaga-Przymus & Lemańczyk, ["B-free sets and dynamics"](https://arxiv.org/abs/1509.08010) (2015)
  - Araújo, Dymek & Kułaga-Przymus, ["B-free integers in number fields and dynamics"](https://arxiv.org/abs/1507.00855) (survey-style introduction)
  - ["Minimality of B-free systems in number fields"](https://arxiv.org/abs/2207.05396) (2022)

**point patterns and spacing statistics**: physics and probability, treating
the 1s as particles.

- gap distributions, pair correlation, clustering. "spacing distribution" is
  literally the term
- **hyperuniformity** (Torquato & Stillinger, 2003) measures how even a point
  pattern is at large scales. euclid patterns are extremely hyperuniform
- **closest to the prime work** (verified): Torquato, Zhang & de
  Courcy-Ireland, ["Uncovering Multiscale Order in the Prime Numbers via Scattering"](https://arxiv.org/abs/1802.10498)
  (_J. Stat. Mech._, 2018) treat the primes as particles, scatter waves off
  them like a crystal, and find hidden multiscale order ("effectively
  limit-periodic"). follow-up: "Hidden Multiscale Order in the Primes"
  (arXiv:1804.06279, not checked)
- related: Montgomery's pair correlation (1973) links prime-zeta-zero spacing
  to random matrix spacing

**aperiodic order / quasicrystals**: patterns that are ordered but never
repeat.

- the 1D Fibonacci chain is a Sturmian word, so euclid spacers live here too
- reference: Baake & Grimm, _Aperiodic Order_ (2013)

**discrepancy theory / uniform distribution**: how far a point set is from
perfectly uniform.

- euclid spacers are the minimum-discrepancy patterns
- reference: Matoušek, _Geometric Discrepancy_; Weyl (1916) on equidistribution

**additive combinatorics**: structure in sets of integers.

- Green–Tao (2004): the primes contain arbitrarily long arithmetic
  progressions
- central theme is **structure vs. randomness**: split a set into a structured
  part and a random-looking part. same idea as the
  [fake number line baseline](#todo-check-patterns-against-fake-number-lines)
- reference: Tao & Vu, _Additive Combinatorics_ (2006)

**applied binary spacing**: spacing patterns built for engineering.

- **Golomb rulers / difference sets**: every gap between points is different
- **low-autocorrelation sequences** (Barker codes, m-sequences): radar, GPS,
  CDMA. reference: Golomb, _Shift Register Sequences_
- **run-length-limited codes**: data storage
- **most-even scheduling**: Tijdeman's "chairman assignment problem" (1980)

### concept map

| spacer concept              | formal home                                                     |
| --------------------------- | --------------------------------------------------------------- |
| spacer                      | binary word / necklace / finite point set                       |
| phase, orientation          | the shift; rotations acting as a group (necklaces)              |
| euclid spacer               | Sturmian / balanced word, minimum discrepancy, maximally even   |
| intervals                   | gap sequence / spacing distribution                             |
| "how even is it?"           | balance, discrepancy, hyperuniformity                           |
| recursive spacer            | hierarchical constructions (substitutions, Toeplitz sequences)  |
| tribe spacer                | a factor of the prime sequence; a point pattern of primes       |
| slot weights across a group | superposition / averaging over an orbit (density, correlation)  |
| fake-prime baseline         | random models (Cramér) and structure vs. randomness             |

### where to start

1. **combinatorics on words + symbolic dynamics**: the closest thing to "the
   general study of binary spacings", and where euclid spacers (Sturmian
   words) and prime-like sets (B-free / Toeplitz sequences) already sit side
   by side
2. **the Torquato primes paper**: quick, visual, and closest to the tribe
   spacer intuition

### continuous perspective

all _from memory_; check before citing.

most of the fields above are discrete, but nearly every one has a continuous
parent. often the discrete pattern is a continuous thing **sampled onto a
grid**. this connects `loop` (continuous) to `spacer` (discrete).

**the common thread: a euclid spacer is a sampled rotation.** move a point
around a circle by k/n of a turn per step and write 1 whenever it passes the
starting mark. the result is the euclidean rhythm. spacer phase is the
starting position, and `relativeSpacerPoints` ([0, 1)) is the continuous view.
in combinatorics on words these are **mechanical / rotation words**. with an
irrational speed the pattern never repeats: the infinite **Sturmian words**.

| discrete field              | continuous counterpart                                                                                                                  |
| --------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| euclid spacers / Sturmian words | a sampled **circle rotation**; or a straight line crossing a grid (a "cutting sequence")                                            |
| maximally even sets         | a regular polygon **rounded to the nearest grid slot**; Tymoczko's continuous **geometry of chord spaces** ("The Geometry of Musical Chords," _Science_, 2006) |
| symbolic dynamics           | **started** continuous: Hadamard (1898) and Morse–Hedlund (1938) coded continuous paths (geodesics) as symbol strings by recording which region they passed through |
| point patterns / spacing    | naturally continuous: points anywhere on the real line (Poisson processes, random-matrix eigenvalues). hyperuniformity is defined there first |
| aperiodic order             | **cut-and-project**: slice a continuous higher-dimensional lattice with a strip and project down to get the discrete pattern             |
| discrepancy                 | Weyl's equidistribution (1916): how evenly n·α (mod 1) fills the interval [0, 1)                                                         |
| additive combinatorics      | continuous versions in ℝⁿ (e.g. the Brunn–Minkowski inequality); heavy use of Fourier analysis                                         |
| primes                      | **analytic number theory**: primes through the continuous zeta function                                                                 |

**bridges worth exploring**

1. **waves → binary: sigma-delta modulation.** how electronics turns a smooth
   signal into 1s and 0s: keep a running error and output 1 whenever it
   overflows. a constant input gives a euclidean / Sturmian pattern. Bresenham
   and error-diffusion dithering use the same mechanism. so: **loop waveform →
   quantizer → spacer**, linking two modules
2. **primes as a sum of waves.** Riemann (1859): the prime count is a smooth
   curve plus one oscillating wave per zero of the zeta function, i.e.
   circles-sines-signals applied to primes. Montgomery's pair correlation
   (above) is about the spacing of those zeros
3. **the Hardy–Littlewood circle method.** the technique behind the k-tuple
   predictions that explain tribe widths. it wraps the integers around a
   circle and studies them with Fourier waves

**summary:** the discrete fields are mostly continuous rotation, motion or
waves sampled onto a grid. the library already has both halves, `loop` and
`spacer`. the bridges between them (sampling, quantizing, rounding) are well
studied and a natural direction to extend.

### from discrete to continuous

the direction of interest here is the **opposite of sampling**: start from
purely synthesized binary patterns (no continuous source) and ask what
continuous objects emerge as they grow toward infinity. no single name; it sits
across the areas below. _from memory_ unless marked otherwise.

#### weight families are autocorrelation

derived, and checked against the README example. `spacerSymmetricSlotWeights`
stacks every orientation of a spacer and counts how many land on each slot.
slot j's weight is the number of points p for which p + j is also a point,
which is the spacer's **cyclic autocorrelation**:

| lag j | points p with p + j also a point (in `[5, [0, 1, 3]]`) | count |
| ----- | ------------------------------------------------------ | ----- |
| 0     | 0, 1, 3                                                | 3     |
| 1     | 0 (→1)                                                 | 1     |
| 2     | 1 (→3), 3 (→0)                                         | 2     |
| 3     | 0 (→3), 3 (→1)                                         | 2     |
| 4     | 1 (→0)                                                 | 1     |

= `[3, 1, 2, 2, 1]`, matching the README. autocorrelation has a large body of
theory, and the Fourier transform of an autocorrelation is a **power
spectrum** (Wiener–Khinchin theorem). weight families are one step from
spectra.

#### 1. limits of measures (equidistribution)

- the simplest way up to the continuous: treat a spacer as point masses on
  [0, 1) (`relativeSpacerPoints`) and ask what they converge to as the
  resolution grows
- euclid spacers converge to perfectly uniform density
- normalized weight families converge to a continuous **density function**
- term: _weak convergence_ of measures

#### 2. spectral and diffraction theory of sequences

- where weight families go in the limit
- Norbert Wiener's _generalized harmonic analysis_ (1930) defines the
  autocorrelation of an infinite sequence and its spectrum
- aperiodic-order researchers call the result the **diffraction measure**:
  what you'd see shining light through the pattern
  - euclid / Sturmian patterns → sharp Bragg peaks ("pure point" diffraction)
  - random patterns → a smooth background
  - Thue–Morse → something stranger in between
- Torquato's primes paper (see above) is this exact analysis applied to primes
- so "the limit of weight families" has a precise answer here

#### 3. automatic sequences and substitutions (pure binary synthesis)

- probably the closest field to "pure binary synthesis": infinite binary
  patterns **generated by rules**
  - "replace 1 with 10 and 0 with 1, repeat" → the Fibonacci word (Sturmian,
    i.e. euclidean)
  - simple automata → Thue–Morse, Rudin–Shapiro
- asks what the infinite results look like: complexity, spectra, limit shapes
- reference: Allouche & Shallit, _Automatic Sequences_ (2003)

#### 4. fractals and self-similar measures

- recursive constructions taken to infinitely many layers and rescaled
  converge to **fractals**
- classic example: keep the outer thirds of `111` → `101`, recurse forever →
  the Cantor set. its running count becomes the continuous **devil's
  staircase** function
- recursive spacers with unboundedly many layers are this kind of object
- terms: _iterated function systems_, _self-similar measures_
- interesting, but not the main target here

#### 5. walks and scaling limits

- turn a binary pattern into a walk: step up on 1, down on 0 (or subtract the
  density), then zoom out
- euclid patterns → a walk that stays near a straight line (why they're "most
  even")
- random patterns → **Brownian motion** in the limit (Donsker's invariance
  principle)
- primes → a walk whose behaviour ties into the Riemann hypothesis
- turns any binary pattern into a continuous curve

#### 6. the whole euclid family: Stern–Brocot and Minkowski ?(x)

- all euclid spacers, indexed by density / resolution = k/n, are organized by
  the **Stern–Brocot tree** (equivalently Farey fractions), built on the same
  Euclidean algorithm as `coreEuclidMap`
- the continuous object attached to that whole tree is **Minkowski's
  question-mark function ?(x)**, a strange continuous function
- the framework for asking about "euclid spacers as a family" in the limit

#### 7. binary rules → a drawn curve

- Thue–Morse, an infinite pattern from pure binary rules, fed to turtle
  graphics (1 = turn, 0 = step forward) **draws the Koch snowflake** (known
  result, roughly mid-2000s)
- pure binary synthesis → a continuous fractal curve, with no sampling

#### summary

| question                                      | where it lives                                                  |
| --------------------------------------------- | --------------------------------------------------------------- |
| synthesizing binary patterns from rules       | automatic sequences, substitutions (Allouche & Shallit)         |
| what spacers converge to                      | weak convergence of measures, equidistribution                  |
| weight families                               | autocorrelation → spectrum (Wiener–Khinchin)                    |
| weight families as n → ∞                      | diffraction / spectral measures (aperiodic order, Wiener 1930)  |
| recursive spacers with infinitely many layers | fractals, self-similar measures, devil's staircase              |
| any pattern → continuous curve                | walks and scaling limits (Donsker)                              |
| the euclid family as a whole                  | Stern–Brocot tree, Minkowski ?(x)                               |
| binary rules → drawn curve                    | turtle graphics (Thue–Morse → Koch snowflake)                   |

#### todo

- **the "logs" memory**: years ago, something like logarithms seemed to emerge
  from euclid spacer family weights, i.e. a continuous function appearing at
  infinity. worth rediscovering: try different families (all densities at one
  resolution, fixed density with varying resolution, spacer groups, lineages,
  cumulative sums, averages over many resolutions)
- take the Fourier transform of weight families (Wiener–Khinchin) and see what
  the spectra converge to

## to explore: values with more dimensions

noted 2026-10-22, while working on weights in `documentation/spacer.md`. a
spacer's cells hold two states, weights hold amounts, and steps carry a sign.
what kinds of number carry even more in a single value, and how could one
symbol show it? everything here is _from memory_ unless marked; check before
citing.

### a ladder of values

| kind            | one value holds                  | where it shows up in the project       | status      |
| --------------- | -------------------------------- | -------------------------------------- | ----------- |
| two states      | in or out                        | a spacer's cells                       | —           |
| natural numbers | how much                         | cell weights (heights)                 | —           |
| integers        | how much, and which way          | signed steps (+5, −3)                  | —           |
| real numbers    | any amount, fractions too        | proportions, refinement limits         | —           |
| complex numbers | a size and an angle              | the circular view (see below)          | from memory |
| quaternions     | 4 parts; rotations in 3D         | —                                      | from memory |
| octonions       | 8 parts                          | —                                      | from memory |

- complex numbers aren't just a pair: they still multiply and divide like
  numbers, and multiplying by one rotates and scales
- Hurwitz's theorem: real numbers, complex numbers, quaternions and octonions
  are the only normed division algebras
- geometric (Clifford) algebra contains complex numbers and quaternions as
  special cases (Hestenes)
- other directions: **dual numbers** (a value and its rate of change; used for
  automatic differentiation), **split-complex numbers** (hyperbolic rotations,
  relativity), **p-adic numbers** (closeness measured by divisibility; maybe
  relevant to the prime module), **values between 0 and 1** (probabilities,
  fuzzy logic: a cell partly filled)

### complex analysis of spacers

- **cells as roots of unity** (derived): cell j of an n-cell circle can be
  written as the complex number e^(2πij/n). rotating a spacer by k cells is
  multiplying by e^(2πik/n), so the circular view's wrap-around arithmetic is
  that multiplication
- **the discrete Fourier transform** of a spacer adds up its points as roots of
  unity. Amiot reads maximally even sets through their Fourier coefficients
  (verified; see the spacer timeline above)
- **weights and spectra**: family weights are autocorrelation, and the Fourier
  transform of an autocorrelation is a power spectrum (Wiener–Khinchin; see
  "from discrete to continuous" above)
- **to look into**: the spectra of weights for patterned versus arbitrary
  spacers; complex-valued cells (an amount and an angle) as objects in their
  own right; what layers and stacks look like in Fourier terms

### symbols that carry several values

- **Bertin, _Semiology of Graphics_** (1967): the visual variables a mark can
  carry (position, size, shape, lightness, colour, orientation, texture), each
  able to carry one dimension. the doc's display form already uses two: fill
  for the state, and shape, size or orientation for the reading
- **heights in one character**: the Unicode eighth blocks ▁▂▃▄▅▆▇█, as in
  Tufte's sparklines. computed 2026-10-22: weights `2 2 0 1 0 3 1 0` become
  `▅▅ ▃ █▃ `
- **a whole spacer in one character**: a braille character is a 2×4 grid of
  dots (256 patterns, U+2800–U+28FF). computed 2026-10-22: with cells 0–3 down
  the left column and 4–7 down the right, `xx...x..` becomes `⠓`. it needs a
  fixed cell order, since braille's own dot numbering isn't in reading order.
  text-mode plotting tools draw with braille this way
- **an angle in one mark**: an arrow (only 8 directions in text) or a colour's
  hue, which is itself a circle. showing complex values with hue for the angle
  and brightness for the size is called **domain colouring**
- so a complex-valued cell could be a single coloured glyph: hue for the angle,
  size or brightness for the amount

### more dimensions

noted 2026-10-22: everything in `documentation/spacer.md` is worked out for one
dimension, and more dimensions are deferred on purpose. the semantics of several
dimensions (neighbours, distance, "the next point", operations, analysis) were
never pinned down. starting points, _from memory_:

- **reading a grid in one dimension**: row by row is simple, but splits
  neighbours apart; following a space-filling curve (Hilbert's, for example)
  keeps neighbours mostly together
- **discrete vectors**: integer steps on a grid, the subject of digital
  geometry; in a grid, which cells count as neighbours is a choice (4 or 8 in
  2D: the von Neumann and Moore neighbourhoods of cellular automata)
- **two dimensions read in one**: a straight line drawn across a 2D grid, read
  as the sequence of grid lines it crosses (a cutting sequence), gives an
  evenly spread spacer, so the sessions' main object is a two-dimensional idea
  read in one dimension (see "continuous perspective" above)
- **shapes of 2D spaces**: each dimension circular or linear gives a flat grid,
  a cylinder or a torus

## reading list

suggested order:

1. ["Jack Douthett and mathematical music theory"](https://www.tandfonline.com/doi/full/10.1080/17459737.2022.2143589) (JMM, 2022), the overview
2. Clough & Douthett, "Maximally Even Sets" (1991), JSTOR doi:10.2307/843811 — [semantic scholar](https://www.semanticscholar.org/paper/Maximally-Even-Sets-Clough-Douthett/1971776b167c91caa28a2e2bb977e31d0e52d5bb)
3. Douthett, "Filtered Point-Symmetry and Dynamical Voice-Leading" (2008)
4. ["Interlocking and Euclidean rhythms"](https://www.researchgate.net/publication/228841028_Interlocking_and_Euclidean_rhythms) (2009)
5. [Douthett & Krantz](https://link.springer.com/article/10.1007/s10878-006-9041-5) (2007)
6. ["The Distance Geometry of Music"](https://arxiv.org/pdf/0705.4085) (2007)

background:

- [Maximal evenness – Wikipedia](https://en.wikipedia.org/wiki/Maximal_evenness)
- [Maximal evenness – Microtonal Encyclopedia](https://microtonal.miraheze.org/wiki/Maximal_evenness)
- [Euclidean rhythms – Chromatone](https://chromatone.center/theory/rhythm/system/euclidean/)
- [Distant Neighbors and Interscalar Contiguities](https://link.springer.com/chapter/10.1007/978-3-030-21392-3_14) (cites Douthett 2008)

## search terms

- "maximally even" + "nth-order" / "second-order" / "iterated"
- "filtered point-symmetry"
- "J-function" maximal evenness
- "Christoffel words" / "Sturmian words" + rhythm
- "euclidean rhythm" + "alternation" / "interlocking"
- "Hügelschäffer" + "generalisation" / "higher order"
- "de La Hire" ellipse construction + offset / eccentric circles
- "polygonal synthesis" / "phaseshaping" / "phase distortion"
- "oscilloscope music" / "XY synthesis"
- "Sturmian words" / "balanced words" / "factor complexity"
- "B-free" dynamics / "squarefree subshift" / "Toeplitz sequence"
- "hyperuniformity" + primes / point patterns
- "discrepancy" + binary sequences
- "mechanical words" / "rotation words" / "cutting sequences"
- "sigma-delta modulation" + Sturmian / balanced sequences
- "explicit formula" primes zeta zeros / "circle method"
- "automatic sequences" / "substitution" + "diffraction" / "spectral measure"
- "generalized harmonic analysis" Wiener / "autocorrelation" infinite sequence
- "Stern–Brocot" / "Minkowski question mark function"
- "roots of unity" + rhythm / necklace / "pitch-class set"
- "discrete Fourier transform" + rhythms / "maximally even"
- "hypercomplex numbers" / "normed division algebra" / "geometric algebra"
- "visual variables" Bertin / "domain coloring" / "sparklines"
- braille + "text-mode" plotting
