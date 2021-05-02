autoscale: true
footer: *© 2021 Justin Spahr-Summers, [available](https://github.com/jspahrsummers/effective-code-review) under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) license*
build-lists: true
theme: Ostrich, 1

# **Effective**
# [fit] Code Review

---

# [fit] [Justin Spahr-Summers](https://jspahrsummers.com)
## _@jspahrsummers_

![right](assets/me.jpg)

[.hide-footer]

---

![inline](assets/logo-facebook.png) ![inline](assets/logo-github.png)

![inline](assets/logo-carthage.png) ![inline](assets/logo-reactivecocoa.png) ![inline](assets/logo-squirrel.png) ![inline](assets/logo-mantle.png)

[.hide-footer]

^ Lifetime total: thousands of code reviews (tens of thousands?)

---

# [fit] Code review puts the **engineering**
# [fit] into **software engineering**

^ Why this talk?

---

# _Poor quality_ reviews...

- Waste everyone's time today
- Waste everyone's time in the future
- Provide a false sense of security

^ Doesn't prevent technical debt, bugs, etc. Will pay for it later, so what's the point?

---

# **Great** reviews...

- Minimize technical debt
- Improve the architecture
- Share domain knowledge
- Provide teaching opportunities

---

# First
# [fit] principles

---

## **Good code review**
## [fit] starts from the same perspective as
## writing **good code**
## [fit] *if we were to write the best version of this, what would it be?*

---

# [fit] Critique the **code**
## *(not the person)*

---

# [fit] **Unblock** others
## [fit] _Increase your team's productivity_

^ One of the most impactful things you can do is unblock your colleagues. PRs sitting open, unreviewed means lower productivity for everyone.

^ I recommend establishing a maximum turnaround time for code reviews (e.g., 24 hours during the working week)

^ If you're having trouble context switching, or bouncing back and forth between reviews and your own coding, establish a few recurring review times as a habit—like before starting your day, after lunch, at the end of your day

---

# [fit] Don't assume it's **obvious**
## [fit] _Code changes and feedback both need explanation_

---

# The
# [fit] process

---

# [fit] _Start at the highest level, then dive deeper..._

1. [Intent](#intent)
1. [Design](#design)
1. [Behavior](#behavior)

---

# _**1.** Intent_
<a name="intent" />

---

# [fit] What is the **goal**?
## _Is the explanation clear?_

---

![](assets/pr-jest-haste-map.png)

[.hide-footer]

^ https://github.com/facebook/jest/pull/896

---

![](assets/pr-jest-haste-map-introduction.png)

[.hide-footer]

^ https://github.com/facebook/jest/pull/896

---

![](assets/pr-jest-haste-map-rationale.png)

[.hide-footer]

^ https://github.com/facebook/jest/pull/896

---

# [fit] Does it **succeed**?
## _How do you know?_

^ Is it tested? How will you know if it regresses?

---

![](assets/pr-jest-worker.png)

[.hide-footer]

^ https://github.com/facebook/jest/pull/4497

---

![fit](assets/pr-jest-worker-performance-test.png)

[.hide-footer]

^ https://github.com/facebook/jest/pull/4497

---

![fit](assets/pr-jest-worker-coverage.png)

[.hide-footer]

^ https://github.com/facebook/jest/pull/4497

---

# _A good summary should include..._

- Background context
- What the bug or feature is
- What the change achieves
- How the change is tested
- Known limitations or anything still missing
- **Request changes** if the summary is incomplete!

---

![](assets/pr-desktop-create-alias.png)

[.hide-footer]

^ https://github.com/desktop/desktop/pull/12000

---

![fit autoplay loop mute](assets/pr-desktop-create-alias-recording.mov)

[.hide-footer]

^ https://github.com/desktop/desktop/pull/12000

---

# [fit] Are the changes
# **appropriate**?

^ Do they align with the explanation in the summary? Are major architectural shifts explained? Do other changes depend on this one? Even incomplete PRs should meet a high quality bar.

---

# _**2.** Design_
<a name="design" />

---

# Ask yourself:
# [fit] How would **you** do it?

^ Try to think through the bug or feature described, and mentally design your own solution for it.
^ If you didn't have this PR in front of you, how would you have done it? Does that highlight any gaps in the code you're reviewing?

---

# Review the **architecture**
### _(The components and how they relate to one another)_

- Do you understand it well enough to use or extend?
- Are the architectural choices justified?
- Would everyone else be happy to maintain this?

---

# Review the **API**
### _(The contract for using each component)_

- Is the API understandable without the PR?
- Does the documentation teach the reader how to use it?
- Is the API conventional?

---

# Is the design **good**?

---

# **[YAGNI](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it)**
# [fit] _You aren't gonna need it_

---

# [fit] **[Simple](https://www.infoq.com/presentations/Simple-Made-Easy/)** vs. easy

---

# **[Pit of success](https://blog.codinghorror.com/falling-into-the-pit-of-success/)**
# [fit] _Make the right things easy_
# [fit] _& the wrong things possible_

---

# **[SOLID](https://en.wikipedia.org/wiki/SOLID)** principles

[.column]

- [Single-responsibility principle](https://en.wikipedia.org/wiki/Single-responsibility_principle)
_each thing should have **only one responsibility**_

- [Open–closed principle](https://en.wikipedia.org/wiki/Open–closed_principle)
_**behavior should be extensible** without modifying code_

- [Liskov substitution principle](https://en.wikipedia.org/wiki/Liskov_substitution_principle)
_**types should be replaceable** with subtypes_

[.column]

- [Interface segregation principle](https://en.wikipedia.org/wiki/Interface_segregation_principle)
_**many specific interfaces** are better than one über-interface_

- [Dependency inversion principle](https://en.wikipedia.org/wiki/Dependency_inversion_principle)
_**depend upon abstractions**, not concrete implementations_

---

# _**3.** Behavior_
<a name="design" />

---

# Review the **tests**

- Tests should be documentation
- Tests should protect against regressions
- Tests should validate the API contract
- Tests need to be understandable
- Are there missing tests?
- You can **request changes**!

---

# Review the **implementation**

- This is the **least important** part to review!
- Would you be able to debug this code?
- [DRY](https://en.wikipedia.org/wiki/Don't_repeat_yourself): Don't repeat yourself
- Don't reinvent the wheel
- Don't ignore linters and warnings
- If it looks convoluted, it's probably wrong

^ Does the code achieve its goal in the simplest, most maintainable way possible?

---

# _But remember..._

1. [Intent](#intent)
1. [Design](#design)
1. [Behavior](#behavior)

---

# Other
# [fit] ProTips™

---

# Give effective **feedback**

- Always **request changes** or **accept**
- Be pragmatic for urgent changes
- Is each stage of review important right now?
- Solicit second opinions
- Prioritize your feedback
- Provide concrete suggestions

---

# **Tell a story** with your commits

- Each commit should build logically upon the previous
- Clean up after yourself:
`git rebase -i`
`hg histedit`
<br />
- [Stack](http://bentrengrove.com/blog/2020/7/8/how-to-stack-prs-in-github) dependent changes

---

# _Questions?_

**Slides and notes** are available at:
[github.com/jspahrsummers/effective-code-review](https://github.com/jspahrsummers/effective-code-review)

Thanks to Lightricks and Barak Yoresh for inviting me to speak!
