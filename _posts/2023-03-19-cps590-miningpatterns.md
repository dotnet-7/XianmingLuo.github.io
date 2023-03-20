---
layout: default
title: Data Science:Mining Patterns
---
# Mining Patterns
## Terminologies
- Alphabet: a set of items
- Transaction: a set of items involved in an activity

## Proofs
### For every frequent itemset, its subset is also frequent.
Definitions: \
$$Def_frequent$$:$$frequent(I)$$ means: $$P(I)\le min\_sup$$ \
$$Def_p$$:$$P(A)=\frac{count(D_A)}{count(D)}, where D is a set of all the transactions in the databse, D_A is a set of all the transactions in the database that contain A$$
Lemmas+Axioms: What does this mean? \
Proof Goal: \
$$\forall A \subset I. frequent(I) \Rightarrow frequent(A)$$ \
Steps:
1. $$\forall A \subset I. frequent(I) \Rightarrow frequent(A)$$ \
Pick any itemset $$A, I$$, where $$A \subset I$$, and...
1.1 $$frequent(I) \Rightarrow frequent(A)$$
Assume frequent(I), and...
1.1.1 $$count(D_A) \le count(D_I)$$
1.1.1.1 $$D_I \subset D_A$$
1.1.1.1.1 $$\forall T \in D_I, T \in D_A$$
Pick any transaction $$T \in D_I$$, and...
1.1.1.1.1.1 $$I \subset T$$
1.1.1.1.1.2 $$A \subset T$$
$$Transitivity_{\subset}$$, 1.1.1.1.1.1, and 1
1.1.1.1.1.3 $$T \in D_A$$
1.1.1.2 $$count(D_A) \le count(D_I)$$
subset property(which?) and 1.1.1.1
1.1.2 $$P(A) \le P(I)$$
1.1.1 and $$Def_p$$
1.1.3 $$P(I) \le min\_sup$$
$$Def_frequent$$ on the assumption of 1.1
1.1.4 $$P(A) \le min\_sup$$
1.1.2, 1.1.3, and transitivity of larger and equal to relation
1.1.5 $$frequent(A)$$
$$Def_frequent$$ with 1.1.4



