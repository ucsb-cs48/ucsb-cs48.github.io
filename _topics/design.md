---
topic: "Design: "
desc: "Waterfall, Agile, Rational Unified Process, etc."
category_prefix: "Design: "
---

# Design: Consensus 

There is a reasonable wide consensus agreement on several ideas related to design:

* Good Design is important to quality software 
* There are Good Designs and Bad Designs
* Design quality is good (vs. bad) when it is
   * Easier to modify in response to changing requirements
   * Easier to test 
   * Easier for new development staff to understand

Regrettably, this is where the consensus ends.

# Design: Controversy

Some areas of controversy:

## Upfront vs. Emergent Design

There is not agreement on whether design is best done:
* Up-front before any coding begins (Waterfall, Rational Unified Process)
* As an emergent property (Agile) through iteration and refactoring

When design is done "up-front", a "design document" is typically produced *before* coding starts.
Example:
   * <https://www.its.dot.gov/research_archives/msaa/pdf/MSAA_SystemDesignFINAL.pdf>
   * <https://blog.tara.ai/software-design-documents/>

## Aspects of design ##

These are not mutually exclusive, but when we talk about design, we might be talking about any or all of these:

* Requirements definition--defining what the user/customer needs or wants
   * How are the requirements are elicited from the user/customer?
   * How are they recorded and communicated to the development team?
   * Is this ongoing, or all up-front, and is there a feedback loop?
* UX design--defining the details of how the customer interacts with the product (the view from the outside)
   * This includes, but is broader than UI design
   * It may also include visual aesthetic aspects (choice of fonts, colors, layout)
   * But also functional aspects (free form text input vs. radio button vs. drop down menu; sequence of user actions, etc.)
* Software Architecture--defining the details of what the code looks like on the inside, i.e.
   * How the code is organized into components 
   * Accordingly, how the developer will interact with the code base

There may also be design aspect in other parts of the Software Development Life Cycle (SDLC) such as QA processes, deployment, release management,
etc.   These may less often come up in the context of *design* discussions, per se, even though there are certainly design aspects involved.

# Wikipedia "textbook" definition of "Design"

The following definitions were retrieved from the opening paragraph 
of the [Wikipedia article on Software Design](https://en.wikipedia.org/wiki/Software_design) on Feb 3, 2019
and are quoted here together with the references cited:

> Software design is the process by which an agent creates a specification of a software artifact, 
> intended to accomplish goals, using a set of primitive components and subject to constraints. [1] 
> 
> Software design may refer to either "all the activity involved in conceptualizing, framing, implementing, commissioning, 
> and ultimately modifying complex systems" or "the activity following requirements specification and before programming, 
> as ... [in] a stylized software engineering process." [2]
>
>
> Software design usually involves problem solving and planning a software solution. 
> This includes both a low-level component and algorithm design and a high-level, architecture design.

One observation about the definition from Freeman and Hart (reference [2]) is that placing design as the "activity following requirements specification and before programming" may well be rooted in a certain
world-view, namely the Waterfall model in which these activities are assumed to be linear.   

This is at odds with the Agile approach in which designs emerge
from starting with the "simplest thing that could possibly work" and the successively refactoring as code smells are replaced with known design patterns through refactoring recipes as recommended by Martin Fowler [3] 

# References

[1]  Ralph, P. and Wand, Y. (2009). A proposal for a formal definition of the design concept. In Lyytinen, K., Loucopoulos, P., Mylopoulos, J., and Robinson, W., editors, Design Requirements Workshop (LNBIP 14), pp. 103–136. Springer-Verlag, p. 109 [doi:10.1007/978-3-540-92966-6_6](https://link.springer.com/chapter/10.1007%2F978-3-540-92966-6_6).

[2] Freeman, Peter; David Hart (2004). "A Science of design for software-intensive systems". Communications of the ACM. 47 (8): 19–21 [20]. [doi:10.1145/1012037.1012054](https://dl.acm.org/citation.cfm?doid=1012037.1012054).

[3] [<i>Refactoring: improving the design of existing code</i>. Addison-Wesley, 2018](https://www.worldcat.org/title/refactoring-improving-the-design-of-existing-code/oclc/989996110&referer=brief_results))

