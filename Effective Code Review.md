autoscale: true
build-lists: true
theme: Ostrich, 1

# **Effective**
# [fit] Code Review

^ Code review is a **key part** of the software engineering process. I'm going to be talking to you today about making code reviews as _effective_ as possible.

---

# [fit] [Justin Spahr-Summers](https://jspahrsummers.com)
## _@jspahrsummers_

![right](assets/me.jpg)

^ Let me introduce myself. My name is Justin Spahr-Summers, a.k.a., [@jspahrsummers](https://jspahrsummers.com) on basically every platform.

---

![inline](assets/logo-facebook.png) ![inline](assets/logo-github.png)

![inline](assets/logo-carthage.png) ![inline](assets/logo-reactivecocoa.png) ![inline](assets/logo-squirrel.png) ![inline](assets/logo-mantle.png)

^ I've worked at smaller companies and larger ones, and been a key contributor to several successful open source projects in the Cocoa community, including Carthage, ReactiveCocoa, the Squirrel update framework, and the Mantle model framework for Objective-C.

^ In total, I've done thousands of code reviews—possibly _tens_ of thousands—in both commercial and open source contexts.

---

# [fit] It's not software **engineering**
# [fit] without code review

^ Why this talk?

^ Code review is _fundamental_ to our discipline. Without code review (and without other key fundamentals like testing, or CS theory), we're just _programming_—not engineering. We elevate our craft, and hold each other to a higher standard, through peer reviews.

---

# _Poor quality_ reviews...

- Waste everyone's time today
- Waste everyone's time in the future
- Provide a false sense of security

^ Many (I daresay _most_) teams are already doing some kind of code review. But if it's not _effective_, what's the point?

^ If your reviews aren't preventing technical debt, aren't improving the end result, and don't provoke good conversation… it's just burning everyone's time for little benefit—and your future self will pay the opportunity cost.

---

# **Great** reviews...

- Minimize technical debt
- Improve the architecture
- Share domain knowledge
- Provide teaching opportunities

^ Submitting a pull request should be the _start_ of a conversation, not the end of one. Use code reviews to achieve all of these goals, and really dig deep into the essence of what you're trying to achieve. The result will benefit everyone!

^ I intentionally do not say that "great reviews prevent bugs." I think bugs should _primarily_ be prevented through type systems, tests, etc., and not rely upon humans to catch before merging. But of course, some reviews will indeed catch some number of bugs.

---

# First
# [fit] principles

^ Hopefully we agree on how _important_ it is to make reviews as effective as possible. Let's now discuss how to do that.

^ Before deciding on or describing any process, I like to identify the first principles. Process should follow principles, not the other way around.

^ For code review, the following are my principles.

---

## **Good code review**
## [fit] starts from the same perspective as
## writing **good code**
## [fit] *if we were to write the best version of this, what would it be?*

^ A great code reviewer is a great engineer, because being able to dig in and understand someone else's code, as well as how to _improve_ it, is required all the time when both writing _and_ reviewing code.

^ _(Note that it doesn't work the other way—not all great engineers are automatically great reviewers! It's an additional skill, and requires commitment to become good at.)_

---

# [fit] **Unblock** others
## [fit] _Increase your team's productivity_

^ One of the most impactful things you can do is unblock your colleagues. PRs sitting open and unreviewed = lower productivity for everyone.

^ I recommend establishing a maximum turnaround time for code reviews (e.g., 24 hours during the working week). If you're having trouble context switching, or bouncing back and forth between reviews and your own coding, establish a few recurring review times as a habit—like before starting your day, after lunch, and at the end of your day.

---

# [fit] Critique the **code**
## *(not the person)*

^ You may have heard the expression, "you are not your code." I prefer "_they_ are not their code."

^ The code that someone submits should not reflect upon them as a person. Code reviews should never make it personal. It should be about the problem we're trying to solve, and whether this particular version of the change is the best way to do it.

---

# [fit] Don't assume it's **obvious**
## [fit] _Code changes and feedback both need explanation_

^ Many, many important things get omitted in communication because someone assumed, "this is obvious, I don't need to say it."

^ **Always** state your assumptions, and overcommunicate. Code authors, explain what you're doing and why! Code reviewers, explain the feedback you're giving, and why it will help!

---

# _The principles_

1. Imagine the best version of the code
1. Unblock your team
1. Criticize code, not people
1. Over-explain everything!

^ So just to recap, my first principles are to:
1. Imagine the best version of the code as the starting point for a review.
2. Unblock teammates and collaborators, to increase everyone's productivity.
3. Focus criticisms on code, and not make things personal.
4. And over-explain _everything_. This goes for authors _and_ reviewers.

---

# The
# [fit] process

^ Okay. Let me share the process that I use now—the one I've honed over thousands of code reviews.

---

# [fit] _Start at the highest level, then dive deeper..._

1. [Intent](#intent)
Goal & summary
1. [Design](#design)
Architecture & API
1. [Behavior](#behavior)
Tests & implementation

^ I approach code reviewing in stages, "outside-in."

^ By starting at the highest level, I can short-circuit my review if I reveal anything that might require a major change to the pull request. There's no point in reviewing each line of code if the architecture is all wrong!

---

# _**1.** Intent_
<a name="intent" />

^ Let's start with the _intention_ behind the change, and make sure we agree on what we want this PR to actually accomplish.

---

# [fit] What is the **goal**?
## _Is the explanation clear?_

^ **Every single pull request summary** should explain the goal of the PR. Otherwise, how will you as a reviewer evaluate it?

^ The author of a pull request will have the _most_ context on the problem, so it's important that they can explain their goal clearly. And if the explanation doesn't make sense to you as a reviewer, **request changes**!

---

![](assets/pr-jest-haste-map.png)

^ Here's an informative pull request summary from [Christoph Nakazawa](https://cpojer.net) on [Jest](https://jestjs.io) (a JavaScript testing framework). Although it could use a bit of structure (e.g., with headings), it's very clear in its goal…

^ _[facebook/jest#896](https://github.com/facebook/jest/pull/896)_

---

![](assets/pr-jest-haste-map-introduction.png)

^ The very first sentence gives you the goal right away—to introduce "a new haste map implementation … which is much more scalable than node-haste." There's some assumed context here, but in this case, it refers to something all of the reviewers will be familiar with.

^ _[facebook/jest#896](https://github.com/facebook/jest/pull/896)_

---

![](assets/pr-jest-haste-map-rationale.png)

^ Then, the summary elaborates on why that goal is important. The existing implementation "isn't well designed and not scalable." "This implementation is attempting to [reduce] startup time."

^ Boom. Reviewers now have enough information to evaluate whether the goal is worth achieving (i.e., if this is the right problem to solve).

^ The next thing to figure out is…

^ _[facebook/jest#896](https://github.com/facebook/jest/pull/896)_

---

# [fit] Does it **succeed**?
## _How do you know?_

^ Presuming you, as the reviewer, understand the goal of the PR, your next responsibility is to determine _whether this pull request actually achieves it_.

^ For example:
- Is the bug fix or new feature tested?
- How will you know if there's a regression?

^ These are a couple of the questions you (the reviewer) should have, and the author should have satisfying answers for you right there in the PR summary.

---

![](assets/pr-jest-worker.png)

^ Here's another pull request on [Jest](https://jestjs.io), from [Miguel Jiménez Esún](https://twitter.com/mjesun). All we need to know from this excerpt is that this is intended to be a performance improvement.

^ _[facebook/jest#4497](https://github.com/facebook/jest/pull/4497)_

---

![fit](assets/pr-jest-worker-performance-test.png)

^ And we have _evidence_ that it succeeds at improving performance, because the PR helpfully includes benchmarking results right in the description—along with the process by which anyone can run their own benchmarks.

^ _[facebook/jest#4497](https://github.com/facebook/jest/pull/4497)_

---

![fit](assets/pr-jest-worker-coverage.png)

^ This same PR description includes a picture of automated test coverage after the change. We have evidence that despite the performance improvement, no functionality has regressed. Great!

^ _[facebook/jest#4497](https://github.com/facebook/jest/pull/4497)_

---

# _A good summary should include..._

- Background context
- What the bug or feature is
- What the change achieves
- How the change is tested
- Known limitations or anything still missing
- **Request changes** if the summary is incomplete!

^ To flesh out the summary a bit more, it should include:
- Background context: any prerequisites necessary to understand the change
- Bug or feature: the goal of the pull request
- What it achieves: similar to, but not the same as, the goal
- How it's tested: include a description of automated tests, or at least a test plan!
- Known limitations: anything that is still to come, so the reviewer gets a picture of where this is heading next

---

![](assets/pr-desktop-create-alias.png)

^ One more illustration, this time a pull request I randomly sampled from [GitHub Desktop](https://desktop.github.com). I like this one because it's adding a new feature to an application, and… what's the best way to demonstrate that that's achieved?

^ _[desktop/desktop#12000](https://github.com/desktop/desktop/pull/12000)_

---

![fit autoplay loop mute](assets/pr-desktop-create-alias-recording.mov)

^ A video recording of that feature being used! The author has virtually let the reviewer try the feature themselves, without having to check out and build the code from scratch.

^ _[desktop/desktop#12000](https://github.com/desktop/desktop/pull/12000)_

---

# [fit] Are the changes & summary
# **complete**?

^ As the last step of understanding _intent_, evaluate (at a glance) whether the changes you see align with the explanation given in the summary. For example, are major architectural shifts explained?

^ Does this PR depend on something else? Does something else depend on this one? _Note that individual changes in a stack should still meet a high quality bar._

---

# _**2.** Design_
<a name="design" />

^ That's our review of the intent. This is a good place to pause and reflect. If the intention of the pull request is not clear, or you disagree with the goal, there's no benefit to reviewing further.

^ But if you're on board so far, now it's time to review the high-level design of the changes.

---

# Ask yourself:
# [fit] How would **you** do it?

^ Try to think through the bug or feature described, and mentally design your own solution for it.

^ If you didn't have this PR in front of you, how would you have done it? Does that highlight any gaps in the design you're reviewing?

---

# Review the **architecture**
### _(The components and how they relate to one another)_

- Are the architectural choices justified?
- Do you understand it well enough to use or extend?
- Would everyone else be happy to maintain this?
- Does the architecture make your life easier?

^ If the author disappeared tomorrow, would the rest of the team be able to (and _want_ to) work with this?

^ Will this be easy to run in production (whatever that means for your codebase)? Generally, simpler architectures are more maintainable.

---

# Review the **API**
### _(The contract for using each component)_

- Is the API understandable?
- Does the documentation teach the reader how to use it?
- Is the API conventional?

^ Put yourself in the shoes of an API consumer. If you didn't have this PR in front of you, would you understand how to use it?

^ Remember the principle of "don't assume it's obvious:" what the author finds self-explanatory is often not the case for others!

^ On _convention_: if in Python, is it Pythonic? If inside a framework with its own conventions, does it match those? Although sometimes necessary, breaking convention introduces cognitive overhead.

---

# Is the design **good**?

^ This is the most subjective part of the whole review, and partly just comes with experience, but here are some heuristics to help you answer this question.

^ Remember these for both quality coding _and_ reviewing!

---

# **[YAGNI](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it)**
# [fit] _You aren't gonna need it_

^ "Always implement things when you actually need them, never when you just foresee that you need them."—Ron Jeffries

^ "Do the simplest thing that could possibly work."

^ In other words, _don't over-engineer things_. Only make the design as complex as you need to for _today's_ requirements, not the hypothetical future.

---

### Easy
### _familiar or approachable_
### <br />
### **Simple**
### **_fewer concepts and concerns_**

^ When the two are in conflict, prioritize simplicity. If something is made _easy_ just for the sake of familiarity, it can actually lead to more complexity, especially if the abstractions are leaky.

^ For example, platforms that try to make concurrency invisible to the developer can end up making things more complex (when bugs inevitably arise), even if it's _easier_ to write the code at first.

^ _See Rich Hickey's presentation, [Simple Made Easy](https://www.infoq.com/presentations/Simple-Made-Easy/), as well as my favorite paper, [Out of the Tar Pit](https://blog.acolyer.org/2015/03/20/out-of-the-tar-pit/)._

---

# **[Pit of success](https://blog.codinghorror.com/falling-into-the-pit-of-success/)**
# [fit] _Make the right things easy_
# [fit] _& the wrong things possible_

^ Does the API help consumers fall into the "pit of success," where doing the _right_ thing is easy?

^ Are the default choices sensible and safe? Whenever there are multiple options, does the documentation clearly explain the consequences of each? Is the developer led toward the "right" choice?

^ It should be _hard_ to mess things up! Strong static typing can help a lot here, by preventing logic errors and offering guidance to the user.

^ APIs like this can still offer escape hatches, to make "wrong" edge cases possible, but it's not a bad thing if they feel hard to use!

---

# **[SOLID](https://en.wikipedia.org/wiki/SOLID)** principles

[.column]

- [Single-responsibility principle](https://en.wikipedia.org/wiki/Single-responsibility_principle)
_each thing should have **only one responsibility**_

- [Open–closed principle](https://en.wikipedia.org/wiki/Open–closed_principle)
_**behavior should be extensible** without modifying code_

- [Liskov substitution principle](https://en.wikipedia.org/wiki/Liskov_substitution_principle)
_**types** should be replaceable with **subtypes**_

[.column]

- [Interface segregation principle](https://en.wikipedia.org/wiki/Interface_segregation_principle)
_**many specific interfaces** are better than one über-interface_

- [Dependency inversion principle](https://en.wikipedia.org/wiki/Dependency_inversion_principle)
_**depend upon abstractions**, not concrete implementations_

^ Classic software engineering principles, still as relevant as ever.

---

# _**3.** Behavior_
<a name="design" />

^ That's our review of the design. It's good to pause again here. If the design has significant flaws that need to be fixed, reviewing the tests and the implementation won't add any value, because they may completely change.

^ If the design mostly looks good, it's time to look at the implemented behavior.

---

# Review the **tests**

- Tests are additional documentation
- Tests should protect against regressions
- Tests should validate the API contract
- Tests need to be understandable
- Are there missing tests?
- You can **request changes**!

^ We'll start from the tests. This is sort of the code review analogue to [test-driven development](https://en.wikipedia.org/wiki/Test-driven_development)—verify that the tests make sense before checking the implementation.

^ Why tests first? They're a form of documentation, and they verify the API does what it says. This means we should look at what the _tests_ are telling us, rather than the implementation.

^ If one of the tests fails, will you know how to investigate and resolve it? Sometimes a simpler test, covering less, is better than a complex test that we cannot hope to debug.

^ Don't wait until later for missing tests—they'll never get added!

---

# Review the **implementation**

- This is the **least important** part to review!
- Be suspicious of convoluted code
- Would you be able to debug this code?
- [DRY](https://en.wikipedia.org/wiki/Don't_repeat_yourself): Don't repeat yourself
- Don't reinvent the wheel
- Don't ignore linters and warnings

^ Finally, the implementation. But this is the least important part to review—correctness issues should mostly be caught through type systems and testing, not human reviewers. If the API contract is sensible and there are tests that validate it, any* implementation fulfilling the contract should be basically OK.

^ Does the code achieve its goal in the simplest, most maintainable way possible?

^ Code doing the same thing multiple times should factor that out.

^ Does something already exist to do (part of) what this implementation is trying to do? If you're a particularly helpful code reviewer, you can even offer suggestions for existing APIs, to help the author out!

^ If it ever feels like we're "swimming upstream," where there are flags and code smells left and right, maybe there's a good reason for it! Dig deeper—something that looks convoluted or hard might actually be the wrong thing to do.

^ _* of course, there are exceptions; e.g., performance requirements, side effects_

---

# _The process_

1. [Intent](#intent)
Goal & summary
1. [Design](#design)
Architecture & API
1. [Behavior](#behavior)
Tests & implementation

^ So just to recap this process…

^ Keep this ordering in mind, so that you can go "outside in." You'll maximize your own efficiency at reviewing changes, and you'll be focusing your feedback on what is _most relevant_ for the PR author.

---

# Other
# [fit] ProTips™

^ Outside of the process itself, here are just a couple of things to add.

---

# Give effective **feedback**

- Always **request changes** or **accept**
- Be pragmatic for urgent changes
- Solicit second opinions
- Prioritize your feedback
- Provide concrete suggestions
- Explain the feedback

^ I've been saying "request changes" a lot. **That does not necessarily mean "you need to change this."** It's really just a signal that the author needs to take some action or provide more information (e.g., respond to a question) before the PR is ready for another review.

^ Likewise, **"accept" does not mean "this is 100% fine."** You can ask the author to make changes before landing, and we should trust each other enough that this will happen.

^ Be pragmatic: don't hold up critical fixes on a technicality.

^ On contentious topics, CC more people and ask them to weigh in! Make it a group decision, with consensus, instead of an argument to a stalemate.

^ Prioritize your feedback according to what you believe is most important to address ("definitely fix this" vs. "take it or leave it"), and provide suggestions for how the author can do that (e.g., "just a suggestion here").

^ Always remember to **justify your feedback**, just like you expect the PR itself to be justified. Point out how changing _this thing here_ will lead to a better specific result, or end up improving _this other thing over here_.

---

# **Tell a story** with your commits

- Each commit should build logically upon the previous
- Clean up after yourself:
`git rebase -i`
`hg histedit`
<br />
- [Stack](http://bentrengrove.com/blog/2020/7/8/how-to-stack-prs-in-github) dependent changes

^ As a pull request _author_, you can apply all of the advice I've given so far as well! You can make your summaries clear and informative; you can evaluate and improve your design, tests, and implementation just like your reviewers will.

^ Here's one more thing that you, as an author, can do to make your reviewers' lives easier. By breaking down your changes into a logical sequence, they can see the individual parts of the change more clearly, and naturally follow the progression to a complete feature or bug fix.

---

[.column]

# _The principles_

1. Imagine the best version of the code
1. Unblock your team
1. Criticize code, not people
1. Over-explain everything!

[.column]

# _The process_

1. [Intent](#intent)
Goal & summary
1. [Design](#design)
Architecture & API
1. [Behavior](#behavior)
Tests & implementation

^ So just to recap, my first principles are to:
1. Imagine the best version of the code as the starting point for a review.
2. Unblock teammates and collaborators, to increase everyone's productivity.
3. Focus criticisms on code, and not make things personal.
4. And over-explain _everything_. This goes for authors _and_ reviewers.

^ And my process is to go _outside-in_, starting by reviewing the intent, then the design, and finishing by reviewing the behavior. I'll stop short at any point if I feel that major changes are necessary—this avoids wasting the reviewer's and the author's time.

---

# _Questions?_

**Slides and notes** available at:
[github.com/jspahrsummers/effective-code-review](https://github.com/jspahrsummers/effective-code-review)

Thanks to
[**Lightricks**](https://www.lightricks.com) and **Barak Yoresh**
for inviting me to speak, and to
[Christoph Nakazawa](https://cpojer.net/) for sending me [some great example PRs](https://twitter.com/cpojer/status/1389375377005453313)!

^ Thank you all! I'm happy to take questions on anything and everything.
