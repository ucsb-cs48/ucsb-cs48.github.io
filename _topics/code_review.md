---
topic: "Code Review"
desc: "A team activity to improve the code base and the product"
---


# Kate Kharitonova's Code Review Considerations and Comments
Source:  <https://team-repo.github.io/> 

* Have a formal style guide.
  * Having an official policy on issues will help in keeping the process impersonal (“Does this follow the style guide” vs. “Who’s opinion on this matters”) and also serves to document what issues should be left to the discretion of the author.
* Be kind, keep it professional.
  * Good engineering happens when everyone is calm and happy.  If you trigger an emotional reaction by making your teammate feel attacked, you’ve reduced team productivity. We are all humans, and humans have emotions.
  * Use phrases “this code does X, which has the following drawbacks...” instead of “you have done X, which is bad”.  The code or design is being reviewed, not the author.
* Explain your reasoning.
  * Cite sources (style guides, references, past reviews, mailing lists, stackoverflow). Be clear on the difference between “this is a bug", “this violates our established guidelines”, "this could be done in less complexity, less code", and “this is just my preference”. Don’t force your preferences - but feel free to explain the reasoning behind them.
* Coding is an exercise in compromise.
  * It is common to end up with multiple different totally valid goals that are in conflict and can’t all be satisfied at onces. Don’t be afraid to pointing these out and invite discussion on the best balance.
* Balance giving explicit directions with just pointing out problems and letting the developer decide.
  * It is likely the case that they have spent more time thinking about the problem and may be aware of additional constraints. Solutions you make up on the fly are not always going to be the right answer - identifying the problem and discussing from there goes a long way.
* Encourage developers to simplify code, have better names, extract named helper functions, or add code comments instead of just explaining the complexity to you. 
  * If you have to ask questions in review for it to be clear, future readers will likely be confused as well.
* Snippets show care. Write a snippet, suggest a better name, avoid passive and non-actionable comments.

# Links

* [Unlearning Toxic Behaviors In A Code Review Culture](https://medium.freecodecamp.org/unlearning-toxic-behaviors-in-a-code-review-culture-b7c295452a3c)
   * By Sandya Sankarram, Software Engineer at SurveyMonkey
   * H/T Kate Kharitonova's <https://team-repo.github.io/> project
* [Your Code Sucks and I Hate You](https://jml.io/pages/your-code-sucks-and-i-hate-you.html)
   * A guide to better code reviews
   * H/T Kate Kharitonova's <https://team-repo.github.io/> project
* [Code Review Dance](https://github.com/hmc-cs121-admin/git-pm-workflow/blob/master/code-review-dance-activity.md)
   * An assignment from Harvey Mudd College's CS121
   * H/T Kate Kharitonova's <https://team-repo.github.io/> project
