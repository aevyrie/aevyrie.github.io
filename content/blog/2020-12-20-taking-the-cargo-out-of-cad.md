+++
title = "Taking the Cargo Cult out of CAD"
[taxonomies] 
tags = [ "CAD" ]
+++

Modern CAD<sup>1</sup> software is amazing. What used to require manual projection techniques, teams of drafters, wooden splines, and and army of [ducks](https://www.core77.com/posts/55368/When-Splines-Were-Physical-Objects), can now be accomplished by a single trained  user on a laptop. Despite the revolution that CAD systems were, they are frustrating for power users and have remained nearly unchanged for three decades despite advances in software and hardware. Somehow we've accepted that *this is how it's always been*, and have relegated ourselves to the fate of slamming our heads on the keyboard when the round features in our model explode when someone adjusts a part file in the next county.

I started using 3D CAD as an intern 12 years ago, and have been using it professionally, personally, and academically ever since. I've used, in descending order of experience, Solidworks, Creo Parametric (Pro/E), Fusion 360, Seimens NX (Unigraphics), and Creo Direct. Like many people who've occupied the CAD-jockey seat, I've found myself in a [flow state](https://en.wikipedia.org/wiki/Flow_(psychology)) of intense focus and enjoyment that's difficult to replicate elsewhere. It's not an understatement to say that I love the process of solid modeling.

It wasn't until I was shipping my first consumer product last year that I really began to question the CAD I've been using for so long. It's no coincidence that I had also started learning [Rust](https://www.rust-lang.org/) to satisfy my longtime curiosity of memory managed "systems" programming languages. Learning Rust and using its tooling gave me equal parts admiration for the complexity involved in shipping real software, frustration at the stagnation we've seen in CAD, and inspiration from Rust's ethos of performance and correctness. As much as software engineers criticize the state of their field, there is something to be said about their willingness to try new tools, and develop processes to both prevent regressions and prove correctness by codifying expected behavior into tests. Not only that, but many of the most successful tools and frameworks are free and open source for the benefit of everyone.



---

1. Computer Aided Design. In this case, I'm specifically referring to the CAD used for mechanical design, sometimes referred to as MCAD.

---

## Where we are

It's rare that design engineers have an understanding of their most used tool; many seem apathetic to understanding how these systems work. CAD is often a topic of shared misery, a *lingua franca* of socialization for design engineers who might otherwise work in completely disparate industries. Yet somehow, engineers are ardent defenders of their CAD platform of choice, like victims of some perverse software-based Stockholm Syndrome.

To be clear, I don't blame those apathetic engineers. Software is eating the world, and everything we use teeters precipitously on mile-high towers of abstraction. No one person could ever fully understand the stacks of abstraction they use every day, and developing an understanding of these CAD tools isn't made easily accessible. However, as engineers, we have a responsibility to hold some level of technical understanding of the tools we use. Maybe that's an unrealistic aspiration. 

### The Cargo Cult

My biggest worry is that without any understanding, we become a [cargo cult](https://en.wikipedia.org/wiki/Cargo_cult), carrying out our mindless rituals so that the geometry kernel Gods may smile down upon their flock, and gift us with topologically correct behavior.

>First, thou shalt close all other running applications. Then thou shalt count to three, no more, no less. Three shall be the number thou shalt count, and the number of the counting shall be three. Four shalt thou not count, neither count thou two, excepting that thou then proceed to three. Five is right out. Once the number three, being the third number, be reached, then clicketh thou "Offset Surface" button. The topological inconsistencies, who, being naughty in My sight, shall snuff it.

CAD is supposed to bring correctness and efficiency to our work. While it succeeds at rendering arbitrary views and sections with inhuman precision, it also results in huge amounts of complexity. While 3D representation is a challenging problem, there is no axiom that says it must always have a poor user experience. Further, there is no reason to believe our current workflows are somehow optimal, or anything more than *good enough* software glued together through [decades of acquisitions](https://web.archive.org/web/20100130061758/http://www.ptc.com/company/history-and-acquisitions.htm).

We've developed techniques like skeleton modeling to limit the reference-spaghetti between parts, and using construction features to build more explicit and robust part interfaces (constraints/joints). Unfortunately, these techniques rely on all future users following them, if they even know what they are. Why aren't these architectural choices am explicit part of our CAD structures?

Why do we still use versioning systems (PDM/PLM) that require exclusive file checkout? Why can't we branch and merge our solid models as we parallel path and converge on solutions? Software engineers figured out [distributed source control](https://en.wikipedia.org/wiki/List_of_version-control_software#Distributed_model) almost three decades ago. 

Why aren't design constraint tests a standard procedure for the part release process, similar to unit and integration tests in software CI? Why isn't design intent *explicitly modeled*, instead of added as fragile hacks in the feature tree? Parametric history-based modeling with B-reps feels like [accidental complexity](https://en.wikipedia.org/wiki/No_Silver_Bullet) to me. Does parametric modeling *really* provide enough benefit to offset the insane reference complexity it adds? How often could your parametric changes be made with simple direct modeling edits without the reference-hell that parametric modeling brings?

Why is the mouse still our primary input method? Drawing is a powerful method of visual communication and understanding, often the first tool we grab when we need to communicate our ideas, yet the digital drafting table is nowhere in sight. What happened to Ivan Sutherland's vision for *Sketchpad*?<sup>2</sup>

The boring, unsatisfying, answer to these (and most) questions is likely cost and risk. Granted, *parts* of these features exist in *some* systems, but the industry as a whole is not taking a critical look at *why* we use this software the way we use it. It feels like the sheer momentum of historical cruft is carrying us along an inevitable and unchanging path. Even [startups](https://onshape.com)<sup>4</sup> aren't taking a de novo approach.

Unlike the current world of software development, the software tools hardware engineers use is almost always proprietary, and rarely shared. 

---

2. More on this soon.

3. There are [some individuals doing incredible things](https://github.com/mkeeter/antimony), but I don't see any commercial entities attempting to advance the state of the art of our workflows. I'd love to be proven wrong.

4. Well, formerly a startup, PTC just bought them. RIP.

---

## Where we've been

I took a drafting class in High School, taught by the prototypical buzz-cut shop teacher and ex-draftsman. At the time I took the class, drafting was a historical curiosity and only existed as the vestige of an aging curriculum<sup>5</sup>. I enrolled because my dad was a draftsman-turned-IT-professional and wouldn't shut up about how he used to walk uphills both ways in the snow while using slide rules and t-squares as skis.

The introduction to hand drafting fundamentals gave me an appreciation for the complexity of accurate representation and communicating intent for a geometric object.

---

5. To be fair, the second half of the class was dedicated to designing a house in AutoCAD. I'm just salty that our rural high school had to cut classes after the community failed to pass a funding referendum. Funding our shitty football team was never a problem though. Priorities.

---


## Where we're headed
