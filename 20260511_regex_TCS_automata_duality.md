# Session Notes — 11 May 2026

**Date:** 11 May 2026
**Aperture:** Regex trainer completion and the machines underneath it
**Where it ended:** Generation-recognition duality in formal language theory; automata as discrete dynamical systems; invitation to drill TCS vocabulary

---

## The Thread

What began as a software project — building and iterating an interactive regex trainer — opened steadily outward. Completing the 140-drill trainer forced the question of why certain patterns work, which forced the question of what regex engines are doing, which led to NFA vs DFA, the Chomsky hierarchy, Turing machines, and complexity theory. The session was architecturally unusual: project work (trainer builds, skill updates, error notebook) and theoretical instruction ran in parallel throughout, rather than the usual pure Socratic thread. The deepest material came at the end: Dinghao's intuition that the accept/reject framing was too thin — that machines should be *generating* something — turned out to point directly at one of the field's central dualities. The session closed on an invitation to drill the formal vocabulary properly, with a new trainer scope defined.

---

## Key Nodes

**Ken Thompson's 1968 regex implementation in ed** — the bridge from Kleene's 1951 pure mathematics to engineering. Thompson built a true NFA engine, still within Type 3. His implementation had the linear-time guarantee; it was Perl (Wall, late 1980s) that knowingly stepped outside regular languages by adding backreferences.

**NFA vs DFA** — not about probabilities; 'nondeterministic' means the machine accepts if *any* choice-path leads to acceptance. DFA: one transition per state per symbol, always linear. NFA: multiple transitions permitted, implemented by backtracking (Perl/Python/JS engines) or parallel-state tracking (Thompson's original). Subset construction theorem: every NFA has an equivalent DFA, at the cost of up to 2ⁿ states.

**Kleene's theorem** — a language is regular if and only if it can be described by a regular expression (in the original mathematical sense: union, concatenation, Kleene star only). The proof is constructive in both directions: Thompson's construction (regex → NFA), state elimination (NFA → regex).

**The Chomsky hierarchy** — four grammar/machine classes: Type 3 (regular, finite automaton), Type 2 (context-free, pushdown automaton), Type 1 (context-sensitive, linear-bounded TM), Type 0 (recursively enumerable, Turing machine). Each strictly more expressive than the one below. Originally Chomsky's 1956-59 tool for characterising natural language grammars; independently equivalent to automata theory.

**Why {aⁿbⁿcⁿ} is Type 1 not Type 2** — a PDA has one stack. After using it to match a's against b's (emptying it), there is nothing left to count b's against c's. Two stacks would give you a Turing machine. Formal proof via the pumping lemma for CFLs.

**Fixed counting vs unbounded counting** — \w{5} is regular because 5 is baked into the machine at compile time (five states in sequence). {aⁿbⁿ} is not regular because n is input-determined; you would need a different machine for each n. The corrected statement: FAs can count to any *fixed* bound; they cannot count to an *input-determined* value.

**aⁿbᵐ is regular; aⁿbⁿ is not** — the key insight Dinghao arrived at independently. Unconstrained repetition (any a's, then any b's) is just a+b+, no dependency required. The equality constraint between n and n is what demands a stack.

**NP-hardness of regex with backreferences** — backreferences push regex outside regular languages (they require memory of what was captured, which a finite automaton cannot have). The matching problem becomes NP-complete; the reduction is from SAT. Cook-Levin theorem (1971) established SAT's NP-completeness. In practice, catastrophic backtracking is the engineering manifestation.

**Generation-recognition duality** — Dinghao's most generative question: why accept/reject when machines should be *computing something*? Answer: function problems and decision problems are computably equivalent (self-reducibility); the decision framing is mathematically cleaner. Chomsky's hierarchy was expressly stated from both sides: generative grammars produce strings, automata accept/reject them — same languages, two operational stances. HMMs as probabilistic generalisations of NFAs (Viterbi ↔ NFA dynamic programming) are the bridge to Dinghao's neuroscience background.

**Automata as discrete dynamical systems** — current state + input → next state, with a terminal verdict replacing the continuous trajectory. The accept/reject output is the discrete analogue of settling into attractor A vs attractor B. This framing closes the gap with Dinghao's native intuition.

---

## Dinghao's Best Contributions

- Spotted independently that aⁿbᵐ (no constraint between exponents) is regular while aⁿbⁿ is not: 'do you mean aⁿbⁿ is impossible BUT aⁿbᵐ is not?' — arrived at the key structural distinction without prompting.

- The generation-recognition tension: 'I always feel that machines are generating/computing something, instead of making a decision on computability' — this wasn't a confusion to be corrected but a genuine intuition pointing at the duality. Treated accordingly.

- Identified the Markov chain parallel for automata, correctly (structural overlap in state-machine architecture; the difference is probabilistic self-driven vs input-driven deterministic).

- On the drill asymmetry: 'I can always skip if I find a question easy enough, but if there are not enough drills I can't make up new ones.' Applied independently to drill design, correctly generalised to life philosophy (不足尚可補，過之無以救, tentatively attributed to Tokugawa Ieyasu — attribution likely apocryphal, sentiment traceable to Laozi ch. 77 and Confucius's 過猶不及).

---

## Threads Parked for Future Sessions

- Full TCS drill trainer: automata, Chomsky hierarchy, computability, P/NP. Handoff prompt written; next step is a new chat invoking the interactive-trainer skill with the TCS scope.

- Catastrophic backtracking in depth: the NP-hardness argument made, the engineering consequences (ReDoS, Stack Overflow 2016 outage) mentioned but not drilled. Worth a section in the TCS trainer or a standalone session.

- How Python code becomes machine instructions: lexing, parsing, AST, bytecode, CPython VM, JIT. Explicitly deferred.

- BNF/EBNF, LR/LL/Earley parsers: introduced but not explored. Natural successor to the CFG material.

- The Chomsky hierarchy and natural language: cross-serial dependencies in Swiss German as argument for context-sensitivity; the political dimension of the 1950s behaviourism debate. Barely touched.

---

## Memorable Lines

'The hierarchy isn't just about strings and formal grammars. It's a taxonomy of the memory structures required to compute different kinds of dependencies.'

'Passing a drill is not the same as having a correct pattern.' (On the ABAB/two-char-repeat conflation that passed because the test string was too easy.)

'fin. now you have no excuse.' (Closing line of the remedial trainer.)
