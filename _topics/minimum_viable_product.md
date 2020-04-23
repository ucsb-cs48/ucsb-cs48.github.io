---
topic: "Minimum Viable Product (MVP)"
desc: "Simplest thing that a customer would actually use"
---

The idea of a **Minimum Viable Product** is to build an implementation of your product idea that is
* **minimal**: as simple as possible, as quick to build as possible
* **viable**: a customer using this product would say: "yes this meets a need I have".

The main idea is to **start getting user feedback as quickly as possible**.

The essence of Agile is "inspect and adapt".  
* You never know if you are building the **right thing**, unless and until you see **real users
using it**.
* You need to get to that stage **as quickly as possible**.

Example:

* A case study: putting a card game online.
* The card game: <https://dl.acm.org/doi/10.1145/3328778.3366989>
  > Colleen M. Lewis and Phillip Conrad. 2020. 
  > **Teaching Practices Game: Interactive Resources for Training Teaching Assistants**. 
  > In Proceedings of the 51st ACM Technical Symposium on Computer Science Education (SIGCSE ’20). 
  > Association for Computing Machinery, New York, NY, USA, 1110–1111. DOI:https://doi.org/10.1145/3328778.3366989

This was intended as a game with actual playing cards, to be played face to face.

Game Rules for each round:

1. One person (the *Leader* for this round) draws a card and reads the bold text at the top.
2. The *Leader* then asks the group: What would you do in that situation? <br>
   Each member of the group answers the question or passes
3. After hearing all answers (or 3 minutes), the *Leader* reads the sample answer from the card aloud and picks their favorite answer (the *Winner* for this round). The *Winner* keeps the card to tally who won the most times.
4. The leader rotates clockwise.

At the end of the game, the overall *Winner* is the one with the most cards.

Some sample cards:

![card01]({{'/topics/minimum_viable_product/card01_25pct.png' | relative_url }})
![card02]({{'/topics/minimum_viable_product/card02_25pct.png' | relative_url }})
![card03]({{'/topics/minimum_viable_product/card03_25pct.png' | relative_url }})
![card04]({{'/topics/minimum_viable_product/card04_25pct.png' | relative_url }})

Proposal: Can we make a webapp that would allow for this training exercise to be done online, during the COVID-19 "All Zoom, all the time" era?

Now, consider these four proposals for building this product.  This illustrates an iterative approach to getting to an MVP.

These are actual excerpts from an email conversation between Phill Conrad and
Colleen Lewis, shared with permission.

# Proposal 1

Email from Phill to Colleen:

> What if I made an online version of the game so that folks could play over Zoom (or other video conference tool)
>
> I.e. a website with the content that
> * allows anywhere from three to six players to join a session
> * keeps track of whose turn it is
> * displays the next card
> * gives each person except the one who's "it" a chance to type in their answer
> * when everyone has typed in an answer reveals them all at the same time
> * allows the person who is "it" to select the winner for that round
>
> I'm teaching a web dev course this quarter and I need a demo project to work on anyway.  
>
> Let me know your thoughts.

# Proposal 2

Collen's reply to Phill:

> That's awesome! If you're up for making it! That's great!
> 
> I can imagine a version - using zoom - where it would be a bit more similar to playing in person. I added some text in bold and crossed out things that then wouldn't be relevant. But - if you have a different vision for it -go for it! 
>
> * allows anywhere from three to six players to join a session 
> * keeps track of whose turn it is
> * displays the next card and people can say what they would do or pass.
> * ~~gives each person except the one who's "it" a chance to type in their answer~~
> * ~~when everyone has typed in an answer reveals them all at the same time~~
> * allows the person who is "it" to select the winner for that round
> * allows the person who is "it" to see the sample answer and read that. 
>
> :-) 
> - Colleen

# Proposal 3

Phill to Colleen:

> Sounds good to me... These revisions make it closer to a "minimum viable product" which is a good thing.   
>
> ...
>
> In fact, it occurs to me that the most minimum viable product is simply a site that would shuffle the deck and display the next card.     Period.
>
> On a video conf session, the host brings this up and does "share screen".   Who is "it" and the rest of game play can simply be done by the participants coordinating verbally.
>
> This makes the app something feasible to deliver very quickly.


# Proposal 4

Colleen to Phill:

> Sure! Totally! That's a great point! I can even [imagine] a minimal viable product is a slide deck that has the questions and then on the next slide the question & answer on it. That would be cool to be able to show a client right away :-) 

# Resolution

Phill back to Colleen

> That's perfect, because it illustrates the point that the MVP doesn't always involve code!

My email to Colleen went on to note [this story from the founders of Doordash](https://www.productdone.com/doordash-concierge-mvp/) about how their initial  MVP was nothing more than a static html page with a list of a few restaurants, each with a link to a scanned pdf of the menu, and a phone number to call.   

From that, you iterate, and only write the code you really need.

The article mentions the idea of a [Concierge MVP](https://www.shortform.com/blog/concierge-mvp/) which is an important idea in an approach known as "Lean Business" (which is a "cousin" of the Agile methodology).   

# "Concierge MVP"

A "Concierge MVP" is a product that:
* you can get set up quickly
* *involves human intervention* in steps that might later be partially or fully automated
* has, as its main purpose: market validation and information gathering
* is not intended as a long-term strategy (and is likely not viable or sustainable long-term)

It's the *human intervention* that defines a "Concierge MVP".

# Final Roadmap for the TA Training Card Game:

Note that you might never build the later iterations; you only keep developing as long as the extra features really, really add value.

* Iteration 1: Powerpoint

* Iteration 2: Webapp that mirrors the Powerpoint functionality.
  * Iteration 2a: Just all the cards in order, with a button to reveal the sample answer.
  * Iteration 2b: Randomizing the cards.

* Iteration 3: Add a way to keep track of who is playing, and whose turn it is to be "it".

* Iteration 4: Add "keeping score".  But the game is still shared over a zoom screen by just one participant.

* Iteration 5: Add folks being "join a game" and see the screen for themselves (eliminating the need for anything more than a shared audio channel).

* Iteration 6: A game that could be completely self contained over text chat that takes place inside the app.   Folks might still *want* to have audio and/or video open, but it would be an enhancement rather than necessary.    


