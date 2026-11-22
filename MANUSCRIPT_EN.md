---
title: 'Exact 1- and 2-Limited Domination Numbers of Generalized Sierpiński Graphs over the Three-Vertex Path'
author: 'Ryutaro Yonezu — Independent Researcher'
date: '2026-11-22'
geometry: margin=25mm
fontsize: 11pt
header-includes:
  - \usepackage{amsmath,amssymb}
---

**Research No.80.**

**Status.** AI-assisted, not human peer reviewed. Independent AI mathematical audit was reported PASS for both main cases by the author, based on auditor responses supplied in the research conversation; those findings are summarized separately as **author-provided audit evidence**, not independently authenticated transcripts. Novelty **INCONCLUSIVE**; references **PARTIAL**. All rights reserved. **Prepared preprint; publication status must be checked against the public record.**

## 1. Definition

Let $P_3$ be the path on the symbols $\{0,1,2\}$ with edges $01$ and $12$. For $n\ge1$, the generalized Sierpiński graph $G_n=S(P_3,n)$ has vertex set $\{0,1,2\}^n$. Distinct words $u,v$ are adjacent if for some index $h$, $u_i=v_i$ for all $i<h$, $\{u_h,v_h\}\in E(P_3)$, and $u_i=v_h$, $v_i=u_h$ for all $i>h$.

A set $D\subseteq V(G)$ is *$k$-limited dominating* when (i) each vertex outside $D$ has a neighbor inside $D$, and (ii) each selected vertex $x\in D$ has at most $k$ neighbors outside $D$. The minimum cardinality is $\gamma_k^L(G)$. This is **not** $k$-tuple domination.

## 2. Main results

**Theorem 1 ($k=1$).** For all integers $n\ge1$,
$$\gamma_1^L(S(P_3,n))=2\cdot 3^{n-1}.$$

**Theorem 2 ($k=2$).** For all integers $n\ge2$,
$$\gamma_2^L(S(P_3,n))=4\cdot 3^{n-2},$$
while $\gamma_2^L(S(P_3,1))=1$.

## 3. Proof of Theorem 1: exact finite-state induction

For $n=1$, choose $D=\{0,2\}$. For $n\ge2$, take all words whose final two symbols are unequal. Each block on a fixed prefix of length $n-2$ contributes six selected words. The three nonselected suffixes $00,11,22$ are dominated within their block. Each selected word has at most one nonselected neighbor within its block; edges between blocks have constant final-two-symbol endpoints and therefore do not touch a selected word. This gives the upper bound $2\cdot3^{n-1}$.

For the lower bound, the three extreme vertices $0^n,1^n,2^n$ are interface vertices. Each takes one of four boundary states: (0) unselected and internally undominated; (1) unselected and internally dominated; (2) selected with zero unselected internal neighbors; (3) selected with one unselected internal neighbor. All nonboundary vertices must already satisfy the full domination and limited-neighbor constraints. Let $c_n(s)$ be the minimal number selected for each feasible three-coordinate interface state $s$.

The graph $G_{n+1}$ is formed from three copies of $G_n$, adding an edge between boundary vertex 1 in copy 0 and boundary vertex 0 in copy 1, and another between boundary vertex 2 in copy 1 and boundary vertex 1 in copy 2. The finite min-plus operator $T$ joins child interface states and validates six internalized boundary vertices. The exact program `verify_k1_finite_state.py` enumerates all candidates and checks the transition. It enumerates $7$, $36$, and $64$ feasible states for $c_1,c_2,c_3$ respectively; $c_4$ also has the **same 64-state support** and $c_4(s)-c_3(s)=36$ for every state.

Let $\delta(s)=c_3(s)-18$. By additive homogeneity on identical state supports, $T(c+K)=T(c)+3K$, so the complete computed identity $T(\delta)=\delta$ is an induction certificate. Hence for $n\ge3$ on the same 64-state support,
$$c_n(s)=2\cdot3^{n-1}+\delta(s).$$
For the **whole graph** (as opposed to a component awaiting an exterior neighbor), boundary state 0 is prohibited. The certified minimum of $\delta$ over these complete states equals zero. The lower bound follows, with $n=1,2$ checked directly. The finite-state table and verifier are part of the frozen original evidence.

## 4. Proof of Theorem 2: a block lower bound

For $n\ge2$, partition $G_n$ into $B=3^{n-2}$ suffix blocks, each of nine vertices $w\{00,01,02,10,11,12,20,21,22\}$. Inside each block write
$$L=\{00,01,02,10\},\qquad R=\{20,21,22,12\},\qquad C=\{11\},$$
with common prefix $w$ suppressed.

**Upper bound.** Select all vertices with suffix in $\{01,10,12,21\}$. Each block has four selected vertices. Its unselected vertices are all dominated internally. Each selected vertex has at most two unselected neighbors. Cross-block edges have endpoints with constant final two symbols $00,11$, or $22$ and hence never add an unselected neighbor to a selected vertex. Thus $\gamma_2^L(G_n)\le4B$.

**Lower bound.** Every valid $D$ contains a selected vertex in $L$ because $02$ can only be dominated by $02$ or $01$; similarly $R$ requires one, to dominate $20$. If exactly one vertex of $L$ is selected, that vertex must be $02$. Indeed, selecting $01$ alone in $L$ leaves its three neighbors $00,02,10$ outside $D$; this violates the $k=2$ condition, and $01$ has no cross-block edge. With only $02$ selected in $L$, $00$ cannot be dominated internally and must have a selected cross-block neighbor. By the defining adjacency rule that neighbor has suffix $11$. The symmetric argument for a one-selected-vertex $R$ requires an external selected $11$ to dominate $22$.

A vertex whose last two symbols are $11$ has at most one cross-block edge: the first differing index of a potential cross-block edge cannot be a position occupied by 1 (where the required exchanged symbol would equal 1); it must be the unique last position before the constant suffix where the symbol differs from 1, if such a position and edge exist. Therefore the required external $11$ vertices for distinct deficient $L/R$ groups are **distinct**. This includes the possibility of no such external edge; then the alleged one-selected-vertex group is impossible. For $n=2$ there are no cross-block edges and consequently no deficient group.

Of the $2B$ groups $L,R$, let $q$ have exactly one selected vertex. The selected count in all these groups is at least $4B-q$. The injection described above requires at least $q$ additionally selected vertices in the disjoint union of the $C$ groups. Therefore $|D|\ge(4B-q)+q=4B$, as needed. For $n=1$, the central vertex alone is 2-limited dominating, and the optimum is one.

## 5. Verification and evidence boundary

The frozen $k=1$ source package contains an exact 64-state fixed-point checker and a separately implemented graph-based finite checker. The $k=2$ frozen package includes the original argument, a tree dynamic program and a second implementation for direct adjacency and finite construction checks. The uploaded audit reports were provided as text in the conversation, not as signed files from a third-party auditor. They report PASS for model, infinite proof and independent finite checks. This publication-preparation package does **not** claim fresh external peer review, formally checked theorem-prover proofs, or validated novelty.

A historical optional $k=3$ output in a tree-DP program must **not** be presented as a theorem: its reported value $3^n$ conflicts with monotonicity $\gamma_3^L\le\gamma_2^L$ for $n\ge2$. Only $k=1,2$ are within scope.

## 6. Prior work and limitations

Božović, *On $k$-limited domination: complexity and Sierpiński graphs* (arXiv:2610.01584, 2026), treats the classical complete-graph-based Sierpiński graphs. *On $k$-limited domination in graphs* (2026, DOI 10.1007/s40314-026-03902-2) gives general bounds, not the exact present two formulas in the theorems compared. *On Generalized Sierpiński Graphs* (arXiv:1509.03047) studies other graph parameters including ordinary domination. Saraswathy--Menon, *Co-Secure Domination in Generalized Sierpiński Graph of Paths* (DOI 10.1142/S0219265924500233), studies a different domination condition; its full text was not verified. No priority claim is made; **NOVELTY INCONCLUSIVE, REFERENCES PARTIAL**.

## References

1. D. Božović, *On $k$-limited domination: complexity and Sierpiński graphs*, arXiv:2610.01584 (2026). https://arxiv.org/abs/2610.01584
2. *On $k$-limited domination in graphs*, Computational and Applied Mathematics (2026), DOI:10.1007/s40314-026-03902-2. https://doi.org/10.1007/s40314-026-03902-2
3. *On Generalized Sierpiński Graphs*, arXiv:1509.03047. https://arxiv.org/abs/1509.03047
4. Saraswathy and Menon, *Co-Secure Domination in Generalized Sierpiński Graph of Paths*, DOI:10.1142/S0219265924500233.

## Rights and publication status

Copyright (c) 2026 Ryutaro Yonezu. **All rights reserved.** This manuscript was prepared as a preprint. Uploading or distributing it requires the author’s explicit authorization. The existence of this file does not establish publication.
