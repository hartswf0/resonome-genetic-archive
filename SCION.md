# SCION — how these instruments breed

A scion is a cutting grafted onto living rootstock. Here the rootstock is any instrument page in
`resonome-genetic-archive`, `smith-deck` or `moto`; the cutting is one rule lifted from another.
This file is the theory the scions were built from, and the recipe for making more.

## I. Shattering

- **Movement vs cow path.** Thirty single-file instruments that each invent their own sliders, their own
  audio bus and their own embed hack, colliding with the habit of forking `-08b`, `-08bb`, `-08bbd`…
  to fix one control at a time.
- **The Lie.** "Each of these is a separate instrument." They are one instrument seen through different
  charts: a field, a Smith chart, a ring. What differs is the *chart*, not the engine.
  - catalyst: `mighty-morph-08bbddeef` — a good engine made unplayable by its own control surface.
  - mirror_moment: `smith-deck/index.html` and `moto/mandalla-deck.html` already speak the same five
    postMessage verbs without knowing it.
  - mentor_force: the Smith chart — a map that turns an unbounded plane into a disc a finger can reach.
  - higher_principle: a control is a coordinate chart on the sound; pick the chart whose geometry *is* the music.
- **Collector purge.**
  - abandoned_objective: "fix the sliders on 08".
  - novelty_bait: one more forked suffix.
  - deceptive_potential: the rotated-range-input bug. A native slider rotated 90° keeps its *unrotated*
    hit box — the control you see is not the control you touch. That gap between drawn chart and touched
    chart is the whole design space: every scion below is a deliberate choice of where to put it.

## II. Living lineage genome

```yaml
axis: SONIC
title: scion-rack
seed: one deck protocol; many charts; any instrument can play any other
ancestral_sources:
  - smith-deck/index.html            # dual-deck host, request-audio-stream handshake
  - smith-deck/smith-968.html        # chart-as-control
  - moto/mandalla.html               # ring sequencer, album crates
  - moto/mandalla-deck.html          # host with same-origin stream bridge
  - resonome-genetic-archive/deep-res-02.html
  - resonome-genetic-archive/mdeck-03.html
  - resonome-genetic-archive/mighty-morph-04..08bbddeef.html
origin_conditions: single-file html, touch-first, no build, github pages same-origin across three repos
epochs:
  - {name: oscillator-toys,      weight: 20, mark: euler/galileo/int-oscillator — one equation, one slider}
  - {name: resonance-field,      weight: 70, mark: deep-res, chladini — the picture is the spectrum}
  - {name: morphic-deck,         weight: 85, mark: mdeck/mbeck/mighty-morph — field + sequencer + crossfade in one page}
  - {name: smith-deck,           weight: 90, mark: impedance chart as playing surface, iframe decks under one mixer}
  - {name: mandalla,             weight: 75, mark: rings + crates; musically deep, hard to operate}
  - {name: suffix-drift,         weight: 15, mark: 08b…08bbddeef — control surface degrades while engine improves}
  - {name: scion,                weight: 100, mark: protocol + rack; hybrids by rule transfer, not by fork}
migration_map:
  - {from: smith-deck, to: resonome, optics: chart rim becomes waveform ring, formalism: Γ=(z-1)/(z+1) drives grain read-head not oscillator, concept: match = dry sample, sound: mismatch heard as comb}
  - {from: resonome,   to: resonome, optics: field is both image and wavetable, formalism: scan heads sample field height, concept: seeing = hearing, sound: sample poured in as excitation}
  - {from: smith-deck, to: moto,     optics: concentric rings become tangent constant-r circles, formalism: equal linear speed ⇒ loop ratio = circumference ratio, concept: geometry is the polyrhythm, sound: shared downbeat at the open-circuit point}
  - {from: moto,       to: moto,     optics: decks get the screen and controls get thumbs, formalism: none, concept: host is a mixer not a debugger, sound: crossfader exists}
  - {from: all,        to: rack,     optics: instruments become tiles, formalism: manifest of normalized params, concept: any param can drive any param, sound: one deck plays another}
jukebox_timeline: [oscillator-toys, resonance-field, morphic-deck, smith-deck, mandalla, suffix-drift, scion]
signature_rules:
  - the playing surface is the largest thing on screen
  - no control without a label and a live value
  - what is drawn is what is touched (no transformed native inputs)
  - first touch sounds within a second
  - every deck exposes window.scionDeck and answers scion-hello
  - a hybrid transfers one governing rule, never a UI
tagline: change the chart, keep the engine
ttl: 1
```

## III. Theory of the system (olog)

**Entities** (provenance, confidence)

- ◻Deck — a page that makes sound and can live in an iframe (smith-968, mandalla; 1.0)
- ◻Host — a page that owns transport and mix for ≥2 decks (smith index, mandalla-deck; 1.0)
- ◻Surface — the drawn chart a finger touches: field, Smith disc, ring set (all; 0.9)
- ◻Param — a named scalar in 0..1 (new, protocol v1; 1.0)
- ◻Manifest — the list of a deck's Params (new; 1.0)
- ◻Stream — the deck's full output as a MediaStream (smith-968 L2001, mandalla L3175; 1.0)
- ◻Sample — an audio buffer with a selected region (mighty-morph, mandalla crates; 0.8)
- ◻Macro — a host control not owned by any deck (new; 1.0)
- ◻Binding — (source, target Param, range, invert) (new; 1.0)

**Morphisms**

- [touch]: Surface → Param* — a gesture sets one or more params
- [sound]: Param* → Stream — the engine
- [announce]: Deck → Manifest — `scion-hello` / `scionDeck.manifest()`
- [emit]: Param → Host — `scion-param`
- [set]: Host × Param → Deck — `scion-set` / `scionDeck.set()`
- [tap]: Deck → Stream — `getStream()` or `request-audio-stream`
- [mix]: Stream × Stream → Stream — crossfade, level, EQ
- [bind]: (Macro | Param) → Param — a Binding applied
- [clock]: Host → Deck — `set-bpm`, `resume-audio`, `suspend-audio`

**Groups**: {Deck, Surface, Sample} = instrument · {Host, Macro, Binding} = rack · {Param, Manifest, Stream} = the seam between them.

**Commutative diagrams**

1. *Hand or host, same sound.* `touch ; sound` = `set ; sound` for the same param value. A deck may not
   keep sound-affecting state that only a pointer event can write.
2. *Seen is heard.* Whatever [tap] returns equals what reaches the speakers when monitor is on;
   `set-monitor false` removes the speaker path only.
3. *Cross-couple is composition.* `emit_A ; bind ; set_B` = a Binding whose source is a Param. A host that
   can do macro→param can do deck→deck with no new machinery.
4. *Nesting closes.* A Host that exposes `scionDeck` is a Deck; `mix` output is a Stream. Racks nest.

**Assumption ledger**

- `<safe>` all three repos stay on one Pages origin (same-origin iframe bridge works).
- `<safe>` three.js r128 from cdnjs stays the only external dependency.
- `<uncertain>` MediaStream over postMessage — unreliable across browsers, so the same-origin bridge is the primary path and postMessage the fallback.
- `<uncertain>` iOS Safari allows one resumed AudioContext per iframe after a gesture in the parent; decks expose `start()` and hosts call it from the gesture handler.
- `<requires-user-decision>` whether the originals should be replaced by their scions or kept beside them (kept beside, for now).
- `<requires-user-decision>` whether album crates in `moto` become a shared sample source for every deck.

**Operational and failure calculus**

- `<scion-hello>` [transforms_into] `<scion-manifest>`; no reply in 1.5 s → deck is *legacy*: mix controls only.
- `<getStream() ok>` [enables] `set-monitor false`; no stream → channel shows *direct*, monitor stays on, nothing doubles, nothing goes silent.
- `<scion-set id unknown>` → ignored, no throw.
- `<no user gesture yet>` [blocks] sound; the one START affordance is the fault response.
- `<sample decode fails>` → keep the previous sample, say so in the readout.

**Change tests**

1. *100× decks.* Sixteen decks in one rack: [mix] is associative and Bindings are per-edge, so the theory
   holds; the limit is one AudioContext + one rAF loop per iframe. Survives as theory, needs a shared-context
   variant in practice (child seed below).
2. *Medium swap: no screen.* Drive a deck purely by `scion-set` from a MIDI controller or another deck.
   Diagram 1 guarantees it sounds the same; any deck that fails this has hidden pointer-only state.
3. *Cross-origin fork.* Someone hosts a deck elsewhere: the bridge is gone, postMessage remains, the channel
   degrades to *direct* rather than breaking.

**Theory → artifact mapping**

| invariant | lives in |
|---|---|
| Param is 0..1 | `scionDeck.set/get`, manifest schema |
| hand = host | each deck routes pointer handlers and `set()` through the same setter |
| seen = heard | one master node feeds both `ctx.destination` and the stream destination |
| surface is largest | layout rules in each file; checked by the critic at 390×844 and 1280×800 |
| degrade, don't fail | rack stream-acquisition ladder |

**Residual human theory.** What no file can hold: which hybrids are *musical*. The protocol makes any
rule transferable; only a player can say that Γ-angle-as-read-position feels like an instrument and some
other mapping feels like a settings page. Volume safety on feedback paths is also a human call.

## IV. How to make another scion

1. Pick a rootstock deck and one *rule* from a different family (not a look, not a widget).
2. Write the rule as one sentence of the form "‹geometric thing on the surface› is ‹musical thing›".
3. Copy the rootstock, keep its engine, replace only the [touch] morphism with the new chart.
4. Route every sound-affecting value through one setter; expose it in the manifest.
5. Add `window.scionDeck`, answer the messages in the protocol, honour `?embed=1`.
6. Drop it in the rack next to its parent and cross-couple them. If the pair is more playable than
   either alone, keep it.
7. Have someone who did not build it play it on a phone before you call it done.

Mutation operators that produced the current set: `transmute_medium` (oscillator → sampler),
`mirror_axis` (field as image ↔ field as wavetable), `simplify_rules` (three arbitrary rings → one
tangent family), `invert_goal` (host that controls decks → decks that control each other).
