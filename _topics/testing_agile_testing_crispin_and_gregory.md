---
topic: "Testing: Agile Testing (Crispin and Gregory)"
desc: "Material from the book by Lisa Crispin and Janet Gregory, Agile Testing: A Practical Guide for Testers and Agile Teams"
category_prefix: "Testing: "
indent: true
---

The book *Agile Testing: A Practical Guide for Testers and Agile Teams* by  Lisa Crispin and Janet Gregory (Addison-Wesley 2009) is used by
the Quality Assurance staff at Procore as a reference.

# Procore Notes on Chapter 8

<em>Note: The following notes are from Procore's discussion of Chapter 8 of that book.  
They were provided by Amir Shahheydari, Staff Quality Assurance Engineer, and are shared with permission. They have been lightly 
edited by Phill Conrad to address minor typos.</em>

This chapter frequently calls for customers to be involved in the process. For us that role is
played by the product owner.  We may also have access to data that the discovery process 
(the process of interacting with the customer) makes available to the engineering team.

# Driving Development with business facing tests

Stories are not meant to start with much information. They are a brief description of desired
functionality and an aid to planning and prioritizing work. The team would take the stories and
garnish them with business-facing tests to define requirements per story. These tests are
also referred to as customer-facing or acceptance tests.

# Agile Testing Quadrants

Acceptance tests in Quadrant 2 are written for each story before coding starts. They help the
team to understand what code to write. Quadrant 1 activities ensure internal quality, maximize
team productivity, and minimize technical debt. Quadrant 2 tests define and verify quality, and
help us know when we’re done. We must also define nonfunctional requirements such as
performance, security and usability. Also make note of scenarios for manual exploratory testing.

# The Requirements Quandary

It’s helpful if some members of the technical team can participate in story-writing sessions so
that they can have input into the functionality stories and help ensure that technical stories are
included as part of the backlog. Developers and testers can also help customer break stories
down to appropriate sizes, suggest alternatives that might be more practical to implement, and
discuss dependencies between stories.

Stories are usually just a sentence that expresses who wants the future, what the feature is, and
why they want it. “As an internet shopper, I need a way to delete items from my shopping cart
so I don't have to buy unwanted items”.

They are only intended as a starting point for an ongoing dialogue between business experts and the
development team. If team members understand what problem the customer is trying to solve,
they can suggest alternatives that might be simpler to use and implement.

In this dialogue between customers and developers, agile teams expand on stories until they
have enough information to write appropriate code. Testers help elicit example and context for
each story, and help customers write story tests. These tests guid programmers as they write
code and help the team know when it has met the customer’s conditions of satisfaction. If the
team has use cases, they can help to supplement the example or coaching test to clarify the
needed functionality.

In Agile we accept that we’ll never understand all of the requirements for a story ahead of time.
After the code that makes the story tests pass is completed, we still need to do more testing.
After customer have a chance to see what the team is delivering, they might have different
ideas about how they want it to work. We must learn as much as we can about our customer's
wants and needs. We need test for post conditions, impact on the system as a whole and
integration with other systems.

# Common language

We can also use our tests to provide a common language that’s understood by both the
development team and the business experts. Real-life examples of desired and underied
behaviors can be expressed so that they’re understood by both the business and technical side.
Acceptance tests also help define the scope that everyone know what is part of the story and
what isn’t.

# Eliciting Requirements
- Ask questions: Start by asking questions. Tests can be especially good at asking a
variety of questions because they are conscious of the big picture, the business-facing
and technical aspect of the story, amd are always thinking of the end user experience.
Types of general questions to ask are:
- Is this story solving a problem?
- If so, what’s the problem we are trying to solve?
- Could we implement a solution that doesn't solve the problem?
- How will the story bring value to the business?
- Who are the end users of the feature?
- What value will they get out of it?
- What will user do right before and right after they use the feature?
- How do we know we’re done with the story?

Questions such as “What is the worst thing that can happen?” or “what’s the best thing
that could happen?” are usually asked by testors.
- Use Examples: ask customer to give you use case scenarios
- Multiple Viewpoints: Different people will write different tests or examples from their
unique perspectives. We’d like to capture as many different viewpoints as we can, so
think about your users. Think about other stakeholders, such as your [SRE and]
production support teams. The examples and test adds up quickly. Do we really have to
turn all of those into executable tests? Not as long as we have the customer to tell us if
the code is working the way they want. Prototyping and designs can be tested before a
single line of code is written.
Conditions of Satisfaction
Product owners can use a checklist format to sort out issues by having a checklist for done
criteria. The book uses an arbitrary example to demonstrate a checklist. This can be the done
criteria list that is in the story template:
* API Updated
* ERP Supported
* Documentation Written
* Unit Tests Written
* Custom Reporting Definitions Updated
* Performance Acceptance Passed
* Product Acceptance Passed
* Design Acceptance Passed
* QA Acceptance Passed
* QA tags for automation

# Ripple Effects

A story drops like a little stone into the application water, and we might not think about what the
resulting waves might run into. 

Make a list of all of the parts of the system that might be affected
by the story. Stories that look small but that impact unexpected areas of the system can come
back to bite you. Make sure your story tests include the less obvious fallouts from implementing
the new functionality.

# Thin Slices, Small Chunks

When the development team estimates new stories, it might find some stories too big. Stories
can be too small as well, and might need to be combined with others or simply treated as tasks.
Ask the product owner to bring the stories to a brainstorming session prior to the first iteration.
Have the product owner and other stakeholders to explain the stories. After you understand
what value each story should deliver and how it fits in the context of the system, you can break
the stories down into small manageable pieces. A smart incremental approach to writing
customer tests that guide development is to start with the “thin slice” that follows a happy path.
The sooner you can build the end-to-end path the sooner you can do meaningful testing, get
feedback, start automating tests, and start exploratory testing.If the tasks of writing customer
tests for a story seem confusing or overwhelming, your team might need to break the story into
smaller chunks.

# How Do We Know We’re Done?

The true test is whether the software’s user can perform the action the story was supposed to
provide. Activities from Quadrant 3 and 4, such as exploratory testing, usability testing, and
performance testing will help us find out. The business users or product owners are the right
people to determine whether every requirement has been delivered. When the tests all pass
and any missed requirements have been identified we are done.

# Tests Mitigate Risk

Customer tests are written not only to define expected behaviors of the code but to manage risk.
Driving development with tests doesn't mean we’ll identify every single requirement up front or
be able to predict perfectly when we’re done. It does give us a chance to identify risks and
mitigate them with executable test cases. However, we should still brainstorm potential events,
the probability they might occur, and the impact on the organization. While we don't want to test
only the happy path, it’s a good place to start. After happy path is known, we can define the
highest risk scenarios.
