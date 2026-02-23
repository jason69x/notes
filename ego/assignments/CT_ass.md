Take-Home Examination 
Concurrency Theory

Group Members:
- Ram Niwas Prajapati
- Anupam Das
- Abhishek Meena

###### Question 1: CCS, LTS Construction, and Behavioral Equivalence

*(a)* ![[ctass_p.png|300]]
**States**
- P - initial state , X=b.0+c.0 , Y=b.0 , Z=0

 **Transitions**
- $P\xrightarrow{a}X$​,  $P\xrightarrow{a}​Y$ , $X\xrightarrow{b}Z$ , $X\xrightarrow{c}Z$ , $Y\xrightarrow{b}Z$

 **Initial State**
- P

*(b)*![[ctass_q.png|300]]
**States**
- Q - initial state , X=b.0+c.0 , Y=b.0 , Z=0

 **Transitions**
- $Q\xrightarrow{a}X$​,  $Q\xrightarrow{a}​Y$ , $X\xrightarrow{b}Z$ , $X\xrightarrow{c}Z$ , $Y\xrightarrow{b}Z$

 **Initial State**
- Q

*(d)*  Yes, P and Q are strongly bisimilar. A bisimulation relation is R = {(P,Q), (b.0+c.0, c.0+b.0), (b.0, b.0), (0,0)}. From P and Q both can do a to reach matching states in R. From b.0+c.0 and c.0+b.0 both can do b and c to 0. From b.0 both do b to 0. 0 has no transitions. All moves are matched, so R is a strong bisimulation and P ~ Q.

*(e)*  Yes, P and Q are weakly bisimilar. Neither process contains any τ-transitions, so weak transitions coincide with strong transitions. Since we already have a strong bisimulation R = {(P,Q), (b.0+c.0, c.0+b.0), (b.0,b.0), (0,0)}, and strong bisimilarity implies weak bisimilarity, it follows directly that P and Q are weakly bisimilar.

*(f)*  There is no difference in this example. Strong bisimulation and weak bisimulation give the same result because neither P nor Q has any τ (internal) transitions. Weak bisimulation only differs from strong bisimulation when τ-transitions are present, since it can abstract from them. Here all transitions are visible (a, b, c), so weak bisimilarity coincides with strong bisimilarity, and P and Q are equivalent under both notions.

###### Question 2: Algorithms for Bisimulation and k-Step Bisimulation

*Given*,
finite labelled transition systems, $T_1 = (S_1,\sum, \to_1)$ and $T_2 = (S_2,\sum, \to_2)$

A relation $R ⊆ S1 × S2$ is a bisimulation if for every $(p, q) ∈ R$:
- for every transition $p\xrightarrow{a}p^{'}$  in $T1$, there exists a transition $q\xrightarrow{a}q^{'}$ in $T2$ such that $(p^′, q^′)\in R$
- symmetrically for transitions from $q$.

A k-step bisimulation is defined similarly, except that single transitions may be matched by
paths of length at most k with the same label sequence.

(a) Maximal bisimulation: start with R₀ = S1 × S2 (all pairs). Iteratively remove pairs that violate the bisimulation conditions. For a pair (p,q), check: for every transition p —a→ p′ in T1 there must exist q —a→ q′ in T2 with (p′,q′) still in the current relation; and symmetrically for transitions from q. If no matching transition exists, delete (p,q). Repeat this refinement until no more pairs can be removed (fixed point). The remaining relation is the maximal bisimulation.

(b) Maximal k-step bisimulation: again start with R₀ = S1 × S2. Now, when checking a pair (p,q), replace single-step matching by path matching: for every transition p —a→ p′ in T1, there must exist in T2 a path q ⇒ q′ of length ≤ k whose label sequence equals a (or more generally the same sequence if transitions are compared stepwise), such that (p′,q′) is in the current relation; and symmetrically. If no such matching path exists, remove (p,q). Iterate refinement until a fixed point is reached. The result is the maximal k-step bisimulation.

(c) Termination: both S1 and S2 are finite, so S1 × S2 is finite. The algorithms only remove pairs and never add new ones. Since a finite set cannot strictly decrease infinitely often, the refinement process must terminate after finitely many steps at a fixed point.

(d) Maximality: both algorithms start from the largest possible relation S1 × S2 and only remove pairs that violate the required conditions. Therefore, any bisimulation (or k-step bisimulation) must be contained in every intermediate relation and hence in the final fixed point. Thus the computed relation contains all other valid (k-step) bisimulations and is therefore maximal.

###### Question 3: QBF and Language Inclusion for NFAs

**Part A: Quantified Boolean Formulae**

Given a QBF Q1x1 … Qnxn . φ in CNF, view assignments to x1,…,xn as words over alphabet {0,1} of length n. Build an NFA A that accepts exactly all words of length n (so L(A)= {0,1}ⁿ). Build an NFA B that accepts exactly those words encoding assignments that make φ true. This can be done by, for each clause, building a small NFA that checks the clause is satisfied at the appropriate positions and then intersecting them (via product construction) so B accepts precisely satisfying assignments. Now handle quantifiers by encoding the alternation into inclusion: interpret universal quantifiers as requiring inclusion to hold for all extensions and existential quantifiers as allowing some branch; this can be implemented by unfolding the quantifier prefix into layers of automata that simulate the game semantics of QBF (existential choices correspond to nondeterministic choices in A, universal choices correspond to branching that must all be contained in B). The final construction ensures that the QBF is true iff every word accepted by A (representing a play consistent with quantifiers) is also accepted by B, i.e., L(A) ⊆ L(B).

**Part B: Language Inclusion for NFAs**

NFA-INCLUSION ≤p QBF-CNF. Given NFAs A and B, L(A) ⊆ L(B) iff there is no word w such that w ∈ L(A) and w ∉ L(B). Equivalently, there is no accepting run of A on w and rejecting behavior of B on the same w. Since NFAs are finite, it suffices to consider words up to exponential length bounded by the product construction; encode the existence of such a counterexample as a QBF: existentially quantify a word w (sequence of symbols) and a run of A, and universally quantify over all possible runs of B on w to ensure none is accepting. The matrix φ encodes consistency with transition relations and acceptance conditions in CNF. The QBF is true iff such a counterexample exists; negate appropriately so that the formula is true exactly when L(A) ⊆ L(B).

**Part C: Reductions**

(a) assignments to variables correspond exactly to words processed by the automata, and the quantifier structure is mirrored by the acceptance game encoded via inclusion, so the QBF is true iff every relevant computation of A is accepted by B, i.e., inclusion holds. 

(b), the QBF encodes precisely the existence of a counterexample word and corresponding accepting/rejecting runs; thus it is satisfiable exactly when inclusion fails. Since both constructions simulate the formal semantics of the original problems and are computable in polynomial time, the reductions are correct.