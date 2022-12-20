---
layout: post
title: "Discrete Math"
subtitle: "MIT-OCW 6.042J & 18.062J"
date: 2022-08-11
author: "Kenshin"
header-img: "img/post-bg/post-bg-default.png"
tags:
  - LearnCS
---

#  Introduction and Proofs

- A proof is a method for ascertaining the truth. Thinking of determining factors:
  - Experimentation & observing, Sampling & counter examples, Judge & jury, Word of God or your boss, Customers( for murchants )
- Inner conviction: There are no bugs in my program!
- Mathematical proof is a verification of a propositon by a chain of logical deductions from a set of axioms
  - Def: A propositon is a statement( declarative sentence ) that is the True or False
  - A predicate is a proposition whose truth depends on the value of a variable
  - Universe of discourse, Quantifier
- RSA, cryptosystems, number theory, factoring
- Goldbach's conjecture, Reimann hypothesis
- An implication p=>q is true if p is F or q is T.
  - i.e. p=>q is false if and only if p is T and q is F, or true implies false.
- An axiom is a proposition that is assumed to be true.
- Axioms should be consistent and complete.
  - A set of axioms is consistent if no proposition can be proved to be both T and F.
  - A set of axioms is complete if it can be used to prove every proposition is either T or F.
- Logical deductions, much more elementary facts
- Good proofs are correct, complete, clear, brief, elegant.

# Induction

- **Proof by contradiction**: To prove P is true, we assume P is False(i.e. not P is T), and then use that hypothesis to derive a falsehood or contradiction.
- Ordinary Induction Def: Let P(n) be a predicate. If P(n) is true and for all natural numbers n, P(n) implies P(n+1) is true, then for all n, natural numbers, P(n) is true.
  - By induction.
  - Let P(n) be the proposition that ...
  - Base case(or Basis step): e.g. P(0) is true.
    - keep in mind with the edge cases
  - Inductive Step: For ... , show that P(n)=>P(n+1) is true. Assume P(n) is true for purposes of induction(or inductive hypothesis).
- Strong Induction Axiom: Let P(n) be any predicate. If P(0) is true, for all n, P(0)&P(1)&...&P(n) implies P(n+1) is true. Then for all n, P(n) is true.
  - e.g. Unstacking game

- Invariant: Proof is always by induction.
- Ordering principle

# Number Theory

- Study of the integers

- The greatest common divisor----The Pulverizer: Euclid's Algorithm

- Encryption, decryption: It's hard to factor a product of 2 large primes.

- Congruency(同余): x is congruent to y modulo n : x≡y (mod n) if and only if n|(x-y)

- The multiplicative inverse of x mod n is a number x^-1^, in {0, 1, ..., n-1} s.t.(such that) x·x^-1^≡1 (mod n)

  - gcd(n, k)=1 iff k has a multiplicative inverse.

    - $gcd(n,k)=1\iff\exist s,t,ns+kt=1$

      $\iff n|(kt-1)\iff kt≡1(mod\ n)$

  - e.g. 2≡3^-1^ (mod 5), 5=5^-1^ (mod 6)

  - 1 refers to that two relatively primes a,b has gcd(a, b)=1, and then 1 is equal to the smallest positive linear conbination of a & b.

- Euler's Totient Function: Φ(n) denotes the number of integers in {1, 2, ..., n-1} that are relatively prime to n.

- Euler's theorem: If gcd(n, k)=1, then $k^{\phi(n)}≡1\ (mod\ n)$.

  - Lemma 1: If gcd(n, k)=1, then ak≡bk (mod n) implies a≡b (mod n).
  - Lemma 2: Suppose that gcd(n, k)=1. Let k~1~, ..., k~r~ in {1, 2, ..., n-1} denote the integers relatively prime to n, in which r=Φ(n). Then, {rem(k~1~·k, n), ..., rem(k~r~·k, n)}={k~1~, ..., k~r~}.
  - ...

- Fermat's little theorum: Supposed p is prime and k in {1, 2, ..., p-1}, then k^p-1^≡1 (mod n).

- RSA algorithm: Rivest, Shamir, Adleman

  - Beforehand: The receiver creates a public key and a secret key

    - Generate two distinct primes p and q. Let n=p*q.
    - Select e s.t. gcd(e, (p-1)*(q-1))=1. The public key is the pair (e, n).
    - Compute d s.t. d*e≡1 (mod (p-1)(q-1)). The secret key is the pair(d, n).

  - Encryption: m'=rem(m^e^, n)

  - Decryption: m=rem(m'^d^, n)

  - m'=rem(m^e^, n)≡m^e^ (mod n)$\implies$m'^d^≡m^ed^ (mod n)

    $\exist$r, e*d=1+r(p-1)(q-1)

    So, m'^d^≡m*m^r(p-1)(q-1)^ (mod n)

    n=pq: m'^d^≡m*m^r(p-1)(q-1)^ (mod p)..

    If m!≡0 (mod p), then m^p-1^≡1 (mod p)

    If m!≡0 (mod q), then m^q-1^≡1 (mod q)

    So, m'^d^≡m (mod p), p|(m'^d^-m)..

    m'^d^=m (mod n), m=rem(m'^d^, n)

- FHE over the integers

# Graph Theory and Coloring

- Def: A graph G is a pair of sets (V,E) where V is a nonempty set of items called vertices or nodes, and E is a set of 2-item subsets of V called edges.
  - Two nodes X~i~ and X~j~ is adjacent if {X~i~, X~j~} is an edge.
  - An edge e={X~i~, X~j~} is incident to X~i~ and X~j~.
  - The number of edges incident to a node is called the degree of the node.
  - A graph is simple if it has no loops or multiple edges.
- Coloring Problem.
- Def: A matching is a subgraph of G where every node has degree 1.
  - A matching is perfect if it has size |V|/2.
  - The weight of a matching M is the sum of the weights on the edges in M.
  - A min-weight mathcing for G is a perfect matching for G with the minimum weight.
  - Given a matching M, x & y form a rogue couple if they prefer each other than their mates in M.
  - A matching is stable if there are no rogue couples.
    - Pf: By contradiction. ... WLOG(without lost of generality)(by symmetry) ...
- Stable Marriage Problem
  - N boys & N girls. Each boy has his own ranked list of all the girls. So does each girl. The goal is to find a perfect matching without rogue couples.
  - Thm1: TMA terminates in <=N^2^+1 days.
    - By contradiction
  - Invariant: If a girl G ever rejected a boy B, then G has a suitor who she prefers to B.
  - Thm2: Everyone is married in TMA.
    - By contradiction
  - Thm3: TMA produces a stable matching.
  - A person's optimal(pessimal) mate is his/her (least) favorite from the realm of possibility.
  - Thm4: TMA marries every boy with his optimal mate.
  - Thm5: TMA marries every girl with her pessimal mate.
- Minimum Spanning Trees
  - A walk is a sequence of vertices that are connected by edges.
  - A path is a walk where all vertices are different.
  - A graph is connected when every pair of vertices in the graph are connected.
  - A closed walk is a walk which starts and ends at the same vertex.
    - If k>=3 and all vertices are different, then it is called cycle.
  - A connected and acyclic graph is called a tree.
  - A leaf is a node with degree 1 in a tree.
  - Lemma: Any connected subgraph of a tree is a tree.
  - A tree with n vertices has n-1 edges.
  - A spanning tree of a connected graph is a subgraph that is a tree with the same vertices as the graph.
  - Thm: Every connected graph has an ST.
  - Alg: 
  - Lemma: Let S consist of the first m edges selected by the Alg. Then there exists MST T=(V, E) for G such that S is a subset of E.
    - By induction
  - Thm: For any connected weighted graph G, the Alg produces MST.
- Communication Network
  - A permutation is a function π from the set 0 to n-1 to the same set such that no two numbers are mapped to the same value.
  - Permutation Routing problem for n: for each i, direct the packet at Input i to output π(i). The path taken is denoted by P~i,π(i)~.
  - The Congestion of the path corresponding to P~0,π(0)~...P~n,π(n)~ is equal to the largest number of paths that pass through a single switch.
  - The congestion of an n-input array is equal to 2.
  - Butterfly network...Benes
  - Thm: The congestion of the N-input Benes Network is equal to 1, when N=2^a^ for some a≥1.
    - By induction: P(a)="The theorem is true for a."
    - Base case: N=2^1^, the smallest benes network
    - Inductive step: Constraint graph
    - Key insight: A 2-coloring of the constraint graph which leads to the best solution of the routing problem
- Euler tours
  - Def: A walk that traverses every edge exactly once and starts and finishes at the same vertex.
  - Thm: If a connected graph has an Euler tour iff every vertex of the graph has even degree.
- Directed graphs(digraphs)
  - Let p~i,j~^(k)^ be equal to the number of directed walks of length k from v~i~ to v~j~, then A^k^={ p~i,j~^(k)^ }
    - By induction: P(k)="The theorem is true for k."
    - Base case: k=1
    - Inductive step: By definition of matrix multiplication
- A digraph G=(V, E) is strongly connected if for all u,v in V, there exists a directed path from u to v in G.
- A directed graph is called a directed acyclic graph(DAG) if it does not contain any directed cycles.
- Tournament graphs
  - Def: A directed Hamiltonian path is a directed walk that visits every vertex exactly once.
  - Thm: Every tournament graph contains a directed Hamiltonian path.
    - By induction: P(n)="Every tournament graph on n nodes contains a Hamiltonian path."
    - Base case: n=1
    - Inductive  step: 
  - Thm: The chicken with highest outdegree is a king.
    - By contradiction
- Relations
  - Def: A relation from a set A to a set B is a subset $R\subseteq A\times B$
  - $(a,b)\in R$ : aRb, a~ ~R~b
  - A relation on A is a subset $R\subseteq A\times A$
  - A relation R on A has properties:
    - 自反性     reflexive if for all x in A, xRx 
    - 对称性     symmetric if for all x & y, xRy implies yRx
    - 反对称性 antisymmetric if xRy and yRx implies x=y
    - 传递性     transitive if xRy and yRz then xRz
  - An equivalence relation is reflexive, symmetric and transitive
    - The equivalence class of $x\in A$ is the set of all elements in A related to x by R : denoted [x]. [x]={y; xRy}
    - A partition of A is a collection of disjoint, non-empty sets $A_1...A_n\subseteq A$, whose union is A.
    - Thm: The equivalence class of an equivalence relation on a set A form a partition of A.
  - A (weak) partial order is reflexive, anti-symmetric and transitive
    - A partial order relation is denoted with ≤ instead of R
  - (A, ≤) is called a partially oedered set or poset
    - A poset is a directed graph with vertex set A and edge set ≤
  - A Hesse diagram for a poset(A, ≤) is a directed graph with vertex set A and edge set ≤ minus all self-loops and edges impied by transitivity
  - Thm: A poset has no directed cycles other than self loops
    - Pf: By contradiction
  - After deleting self loops from a poset makes a DAG.
  - a and b are incomparable if neither a ≤ b nor b ≤ a
  - a and b are comparable if either a ≤ b or b ≤ a
  - A total order is a partial order in which every pair of elements is comparable
- A total order consistent with a partial order is called a topological sort. A top-sort of a poset (A, ≤) is a total order (A, ≤~T~) such that $≤\subseteq ≤_T$ 
  - Thm: Every finite poset has a topological sort.

# Sums and Asymptotics

- Perturbation method
- Integration bounds for ∑f(i) when f is a positive increasing function.
- nth Harmonic number H~n~=∑1/i   ~  ln(n)
- Stirling's Formula: $n!=(n/e)^n\sqrt{2\pi n}*e^{\epsilon(n)} , where\ 1/(12n+1)≤\epsilon (n)≤1/(12n)$
- Asymptotic Notation
  - tilde f(x)~g(x) lim f(x)/g(x)=1
  - upper bound f(x)=O(g(x)) lim f(x)/g(x)<∞
  - lower bound f(x)=Ω(g(x)) lim f(x)/g(x)>0
  - f(x)=θ(g(x)) lim f(x)/g(x)>0&<∞
  - f(x)=o(g(x)) lim f(x)/g(x)=0
  - f(x)=ω(g(x)) lim f(x)/g(x)=∞

# Divide and Conquer

- Substitution(Guess & Verify)
- Plug & Chug
- Akra & Bazzi Thm: ![image-20210415230636931](/img/in-post/cs_learning/2022-08-11-discrete-math.png)
- If g(x)=θ(x^t^) for t ≥ 0, and ∑a~i~b~i~^t^<1, then T(x)=θ(g(x))

# Linear Recurrences

- solution space
- determine the constant factors: boundry conditions
- characteristic equation
-  homogeneous recurrence→particular solution→boundry conditions

# Counting Rules

- The cardinality of a set S is the number of elements in S, denoted |S|
- A sequence is an ordered collection of elements (components/terms) not necessarily distinct.
- A permutation of s set S is a sequence containing everty element in S exactly once
- A function f:x→y is a relation between X and Y s.t. every element of X is related to exact one element of Y.
  - surjective: every element of Y is mapped to at least once
  - injective: at most
  - bijective: exactly
- Generalized pigeon hole principle: If |X|>k|Y|, then for all f:x→y, there exists k+1different elements of X mapped to the same element in Y
- A k-to-1 function f:x→y maps exactly k elements of X to every element of Y
  - Division Rule: If f is k-to-1, then |X|=k|Y|
  - Generalized Product Rule: Let S be a set of length k sequences. If there are n1 possible first entries, n2 possible second entries for each first entry..., then |S|=n1*n2*...
  - Sum Rule: If the sets A1..., An are disjoint sets, then |A1...An|=|A1|+...+|An|
  - Bookkeeper Rule: Distinct copies of letters l1, l2, ..., lk, then the number of sequences with n1 copies of l1,..., nk copies of lk, multinomial coefficient![img](https://bkimg.cdn.bcebos.com/formula/fade7c9a55d37c633b1fe98194ff9049.svg)
  - Subset Rule: The number of k element subsets of an n element set is nCk

- Inclusion-Exclusion principle
- Combinatorial Proofs

# Probability

