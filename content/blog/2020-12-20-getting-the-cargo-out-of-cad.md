+++
title = "Getting the Cargo Cult out of CAD"
[taxonomies] 
tags = [ "CAD" ]
+++

Modern CAD<sup>1</sup> software is amazing. What used to require manual [projection techniques](https://en.wikipedia.org/wiki/Multiview_projection), teams of drafters, wooden splines, and and army of [ducks](https://www.core77.com/posts/55368/When-Splines-Were-Physical-Objects), can now be accomplished by a single trained  user on a laptop. Despite the revolution that CAD systems were, they are frustrating for power users and have remained nearly unchanged for three decades despite advances in software and hardware. Somehow we've accepted that *this is how it's always been*, and have relegated ourselves to the fate of slamming our heads on the keyboard when we're greeted with a *Fatal Error Encountered* dialog.


---

1. Computer Aided Design. In this case, I'm specifically referring to the CAD used for mechanical design.

---

## Where we are

It's rare that design engineers have an understanding of their most used tool; many seem apathetic to understanding how these systems work. Yet somehow, engineers are ardent defenders of their CAD platform of choice, like victims of some perverse software-based Stockholm Syndrome.

To be clear, I don't blame those apathetic engineers. Software is eating the world, and everything we use teeters precipitously on mile-high towers of abstraction. No one person could ever fully understand the stacks of abstraction they use every day, and developing an understanding of these CAD tools isn't made easily accessible. However, as engineers, we have a responsibility to holding some level of technical understanding of the tools we use. Maybe that's an unrealistic aspiration. 

My biggest worry is that without any understanding, we become a [cargo cult](https://en.wikipedia.org/wiki/Cargo_cult), carrying out our rituals so the STEP conversion Gods may smile down upon their flock and gift us with topologically correct files:

>First, thou shalt close Microsoft Excel. Then thou shalt count to three, no more, no less. Three shall be the number thou shalt count, and the number of the counting shall be three. Four shalt thou not count, neither count thou two, excepting that thou then proceed to three. Five is right out. Once the number three, being the third number, be reached, then clicketh thou "Export as STEP" button. The topological inconsistencies, who, being naughty in My sight, shall snuff it.

Why do we still use versioning systems (PDM/PLM) that require exclusive file checkout? Why can't we branch and merge our solid models as we parallel path and converge on solutions? Software engineers figured out [distributed source control](https://en.wikipedia.org/wiki/List_of_version-control_software#Distributed_model) almost three decades ago. 

Why aren't design constraint tests a standard procedure for the part release process? Why isn't design intent *explicitly modeled*, instead of added as fragile hacks in the feature tree? Parametric history-based modeling with B-reps feels like a textbook example of [accidental complexity](https://en.wikipedia.org/wiki/No_Silver_Bullet). Does parametric modeling *really* provide enough benefit to offset the insane complexity it adds? How often could your parametric changes be made with simple direct modeling edits instead, without the reference-hell that parametric modeling brings?

Why is the mouse still our primary input method? Drawing is a powerful method of visual communication and understanding, often the first tool we grab when we need to communicate our ideas, yet the digital drafting table is nowhere in sight.

The boring, unsatisfying, answer to these questions is cost and risk. Granted, *parts* of these things exist in *some* systems, but no one<sup>2</sup> seems to be taking a critical look at *why* we use this software the way we use it. It feels like the sheer momentum of historical cruft is carrying us along an inevitable and unchanging path. Even [startups](https://onshape.com)<sup>3</sup> aren't taking a de novo approach.

---

2. There are [some individuals doing incredible things](https://github.com/mkeeter/antimony), but I don't see any commercial entities attempting to advance the state of the art of our workflows. I'd love to be proven wrong.

3. Well, until PTC bought them. RIP.

---

<br>

## Where we've been

I took a drafting class in High School, taught by the prototypical buzz-cut shop teacher and ex-draftsman. At the time I took the class, drafting was a historical curiosity and only existed as the vestige of an aging curriculum<sup>4</sup>. I enrolled because my dad was a draftsman-turned-IT-professional and wouldn't shut up about how he used to walk uphills both ways in the snow while using slide rules and t-squares as skis.

The introduction to hand drafting fundamentals gave me an appreciation for the complexity of accurate representation and communicating intent for a geometric object.

---

4. That's not to say I don't see the value in learning to draft the old-fashioned way, but it felt out of place in the trades and job-skills section of our school's curriculum. 

---








