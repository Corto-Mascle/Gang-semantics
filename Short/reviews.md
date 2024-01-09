We thank the reviewers for all the useful comments. We will do our best to incorporate your remarks into our paper. 

# reviewer 2
Indeed, the reachability problem (called parameterized target problem in the paper) with one register is an interesting open question that we are working on.

# reviewer 3
Many thanks for the detailed review. Indeed, the proofs in the main part of the paper are very sketchy because of the space constraint. 

About Example 4: Indeed i=j does not have to be true.
About Figure 3: You are right, both options are valid unfolding trees and the option you mentioned actually matches Example 15 better, we will change that.
About Lemma 18: The < should indeed be a <=. 
About Lemma 19: Also a small mistake, we should have stated that one can make the coverability objective a specification: to do so, we can add a boadcast loop of a special message on qf.  
About Proposition 20: Actually, we should have written g : n -> n (psi(n,n) +1). The direct induction is then straightforward since |spec(mu_{i+1})| <= g(max(|spec(mu_i)|,n)).
We bound the length of every branch by h(n), a function independent of the branch considered. This proves that the depth of the tree is indeed at most h(n). 
To go from (psi(n,r)+1) x (psi(n,r)+1)^(d+1) + (psi(n,r)+1)^(d+1) to h'(n), it suffices to oberve that d <= h(n)-1 (depth starting at 0). 
About Example 22: the m6 should be a m4, sorry about that mistake. 
About Lemma 26: The high-level idea of the proof is to identify useful broadcasts in the local run and to shorten intermediate parts between these broadcasts using Lemma 25. See Lemma 36 in the appendix for more details.  
About Lemma 27: altmax is always positive. 
