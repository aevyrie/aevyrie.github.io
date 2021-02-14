+++
title = "Towards Better CAD"
draft = false
[taxonomies] 
tags = [ "CAD" ]
+++

Modern CAD<sup>1</sup> software is amazing. What used to require manual projection techniques, teams of drafters, wooden splines, and and army of [ducks](https://www.core77.com/posts/55368/When-Splines-Were-Physical-Objects), can now be accomplished by a single trained  user on a laptop. Despite the revolution that CAD systems were, they are frustrating for power users and have remained nearly unchanged for three decades despite advances in software and hardware. Somehow, we've accepted that *this is how it's always been*, and have relegated ourselves to the fate of slamming our heads on the keyboard when the fillet feature explodes because someone in the next county touched a part file.

I started using 3D CAD as an intern 12 years ago, and have been using it professionally, personally, and academically ever since. I've used, in descending order of experience, Solidworks, Creo Parametric (Pro/E), Fusion 360, Seimens NX (Unigraphics), and Creo Direct. Like many people who've occupied the CAD-jockey seat, I've found myself in a [flow state](https://en.wikipedia.org/wiki/Flow_(psychology)) of intense focus and enjoyment that's difficult to replicate elsewhere. It's not an understatement to say that I love the process of solid modeling. 

However, I don't love the process of fighting with modeling software when hard-to-debug topological problems manifest. I don't love how absolutely *fragile* parametric CAD, and more generally B-reps, feel. I really don't love that those things have been constants for decades, with no end in sight.

This article, then, is the perspective of an experienced, frustrated CAD user. It's also aspirational. I hope to show you where we are, where we've been, and where I *hope* we're going.

---

1. Computer Aided Design. In this case, I'm specifically referring to the CAD used for mechanical design, sometimes referred to as mCAD.

---

# Where we are

It wasn't until I was shipping my first consumer product last year that I really began to question the CAD I've been using for so long. It's no coincidence that I had just started learning [Rust](https://www.rust-lang.org/) to satisfy my longtime curiosity of memory managed "systems" programming languages. Learning Rust and using Git for the first time gave me equal parts admiration for the complexity involved in shipping real software, frustration at the stagnation we've seen in CAD, and inspiration from Rust's aspirations of achieving robustness without compromising performance or composability.

As much as I see software engineers criticizing the state of their field, there is something to be said about their willingness to try new tools, and develop processes to both prevent regressions and prove correctness. Not only that, but many of the most prominent tools and frameworks are free and open source, even in the commercial space! Despite the fact that you can draw parallels between the design of digital and physical systems, the tooling we use for mechanical design feels downright archaic in comparison.

## A Cargo Cult

It's rare that design engineers have an understanding of their most used design tool; many seem apathetic to understanding how CAD systems work. CAD is often a topic of shared misery, the *lingua franca* of despair for design engineers who might otherwise work in completely disparate industries. Yet somehow, engineers are ardent defenders of their CAD platform of choice, like victims of some perverse software-based Stockholm Syndrome.

To be clear, I don't blame those engineers apathetic to understanding their CAD tools. Software is eating the world, and everything we use teeters precipitously on mile-high towers of abstraction. No one person could ever fully understand the stacks of abstraction they use every day, and developing an understanding of these CAD tools isn't easily accessible. However, as engineers, I think we have a responsibility to hold a basic level of technical understanding of the tools we rely on. Maybe that's an unrealistic aspiration.

I worry that without *understanding*, we become a [cargo cult](https://en.wikipedia.org/wiki/Cargo_cult), carrying out our mindless rituals so that the geometry kernel Gods may smile down upon their flock, and gift us with topologically correct behavior.

>First, thou shalt close all other running applications. Then thou shalt count to three, no more, no less. Three shall be the number thou shalt count, and the number of the counting shall be three. Four shalt thou not count, neither count thou two, excepting that thou then proceed to three. Five is right out. Once the number three, being the third number, be reached, then clicketh thou "Offset Surface" button. The topological inconsistencies, who, being naughty in My sight, shall snuff it.

Without understanding, we also lose the ability to meaningfully discuss or improve our tools. It has taken me months of research to get to the point where I feel even remotely comfortable writing about these topics. That's not due to the complexity of the topics though. There is a dearth of mid-level technical material on how CAD works. One one end, I came across a multitude of industry-sponsored blog posts that explained CAD software in the same terms I'd use with a first-time computer user. On the other end of the spectrum I was following links from [Wikipedia's page on Boundary Representation](https://en.wikipedia.org/wiki/Boundary_representation), to MIT OpenCourseWare classes such as [Computational Geometry](https://ocw.mit.edu/courses/mechanical-engineering/2-158j-computational-geometry-spring-2003/), or reading through the STEP, IGES, and Parasolid file format specification documents.

I should probably be the change I want to see in the world, stop complaining, and start contributing; but this is not that article. Instead, I'd like to question why this material doesn't exist. My best guess? The world of hardware development has a culture of proprietary knowledge. To the single software developer reading this post: imagine web development, except every company involved in infrastructure or tooling was either (a) owned by Oracle, or (b) aspired to become the next Oracle. After breifly dipping my toes into the jacuzzi with swim-up bar that is open source software engineering learning material, trying to learn about CAD software systems is like diving into a wave pool of molten salt.

## The Bad and the Ugly

CAD is supposed to bring correctness and efficiency to our work. While it succeeds at rendering arbitrary views and sections with inhuman precision, it also results in huge amounts of complexity. While 3D representation is a challenging problem, I'm not aware of any axiom that states it must always have a poor user experience. Further, there is no reason to believe our current workflows are somehow optimal, or anything more than *good enough* software glued together through [decades of acquisitions](https://web.archive.org/web/20100130061758/http://www.ptc.com/company/history-and-acquisitions.htm).

We've developed techniques like skeleton modeling to limit the reference-spaghetti between parts, and the use of construction features to build more robust part interfaces. Unfortunately, these techniques rely on all future users following them, if the users even know what they are. Why aren't these architectural choices an explicit part of our CAD structures?

Why do we still use versioning systems (PDM/PLM) that require exclusive file checkout? Why can't we branch and merge our solid models as we parallel path and converge on solutions? Software engineers figured out [distributed source control](https://en.wikipedia.org/wiki/List_of_version-control_software#Distributed_model) almost three decades ago.

Why aren't design constraint verifications a standard procedure for the part release process, similar to unit and integration tests in software CI? Why isn't design intent *explicitly modeled*, instead of added as fragile hacks in the feature tree? Parametric history-based modeling with B-reps feels like [accidental complexity](https://en.wikipedia.org/wiki/No_Silver_Bullet) to me. Does parametric modeling *really* provide enough benefit to offset the insane reference complexity it adds? How often could your parametric changes be made with direct modeling edits without the reference-hell that parametric modeling brings?

Why is the mouse still our primary input method? Drawing is a powerful method of visual communication and understanding, often the first tool we grab when we need to communicate our ideas, yet the digital drafting table is nowhere in sight.

The boring, unsatisfying, answer to these (and most) questions is likely cost and risk<sup>2</sup>. Granted, *parts* of these features exist in *some* systems, but the industry as a whole doesn't seem to be taking a critical look at *why* we use this software the way we use it. It feels like the sheer momentum of historical cruft is carrying us down an inevitable and unchanging path of developing incremental improvements to proprietary b-rep kernels. Even startups like OnShape<sup>3</sup> aren't taking a truly de novo approach.

What if we took a step back and rethought CAD? Maybe we could take these frustrations, the lessons of the past, and steal some ideas from the software engineers to make something better.

---

2. Yes, these are hard problems, but I'd wager those are the problems that software engineers *want* to be working on. Cost and risk are (probably) the reasons most of them aren't.

3. Well, formerly a startup, PTC bought them. RIP.

---

# Where we've been

Engineering design has a very long and storied history to which I can't do justice. If it's something you're interested in, I'd highly recommend reading [The Engineering Design Revolution](http://cadhistory.net/) by David E. Weisberg, available online for free. Instead of retreading that ground, I'd like to focus on some of the key developments in the history of CAD. This is important: it gives context to the systems we use today, as well as perspective of what is needed to see any meaningful change in the future.

One of the most amazing CAD presentations I've seen is the demonstration of Ivan Sutherland's [*Sketchpad*](https://www.youtube.com/watch?v=6orsmFndx_o
) from 1963. It's incredible for so many reasons; what I find most extraordinary is that Sutherland came up with the concepts in Sketchpad without prior art, yet they are *still* fundamental to the CAD we use 60 years later:

- Cursor input for direct geometry manipulation
- Drawing 2D primitives in a sketching environment
- Sketch constraints
- Master modeling
- 3D projections
- Solid model representation
- Orbit camera with hidden line removal

Yet this was running on a computer built in 1958, predating integrated circuits, with less than 300 bytes of memory! In the 60 years following Sutherland's thesis there were significant advancements in 3D representation and 2D drafting workflows, but you could argue they are all  derivative of Sutherland's initial vision in some way.

3d rep
breps voxels?

step iges file formats

solid modeling

implicit vs implicit (procedural) storage format

As a natural extension of hand drafting and having much lower computing requirements, 2D CAD reigned supreme until the 90's. Solid modeling began to take off with the advent of intuitive parametric modeling environments introduced by PTC Pro/ENGINEER and further refined by SolidWorks.


direct and parametric modeling


# Where we're headed

I took a drafting class in High School, taught by the prototypical shop teacher and ex-draftsman. At the time I took the class, drafting was a historical curiosity and only existed as the vestige of an aging curriculum<sup>4</sup>. I enrolled because my dad was a draftsman-turned-IT-professional and wouldn't shut up about how he used to walk uphills both ways in the snow while using slide rules and t-squares as skis.

The introduction to hand drafting fundamentals gave me an appreciation for the complexity of accurate representation and communicating intent for a geometric object. The transition from hand drafting to 2D CAD is an incremental improvement. It trivializes many of the annoying bits, such as making perfect lines and arcs, duplicating geometry, and the ability to undo. However, it doesn't make any guarantees about the correctness of your 2D projections of a 3D object. In that way, 3D CAD takes the drafting process to a different level of abstraction. Instead of focusing on the 2D output, you can focus on the 3D source of truth from which drawing views can be trivially derived.

This is where we are now. We spend our time attempting to produce an accurate geometric representation of the intangible object living in our mind or in graphite on our sketchpads. Often times we find that the thing we imagined doesn't quite obey the rules of Euclidean space, and we work out the details as we model something in CAD that does. With complex parts, especially those with ID-driven surfaces, we often end up spending more time fighting b-reps than producing useful work.

I would argue that the next leap in abstraction would allow us to once again focus more time on the engineering than the drafting. The parts and assemblies we make are a means to an end. There exists an infinite number of variations within our multi-dimensional design space, which is bounded by our design constraints. Our goal is to attempt to minimize or maximize certain characteristics of a solution within this design space to choose an optimal (or more realistically: good enough) design.

generative design as it currently exists isn't the solution
    Needs more human feedback in the process
    Can over-fit to meet a specific set of criteria, e.g. minimizing stress and mass
    some criteria we want to min/max are difficult to quantify
    generative design should help remove the rote parts of the work so we can focus on the parts of design that matter

Optimal designs don't exist as a single continuous region of the design space, there are local optima that aren't necessarily connected globally along the same gradient. You can't start from any design and reach the optimum through incremental improvements. It can take creative thinking to leap these valleys to find another optimal but disparate "peak" in the design space.

Being able to describe the problem in terms of first principals to the computer so it can assist us in exploring the global problem space would empower engineers to do much better work by more quickly identifying many local optima. This would help prevent us from fixating on and optimizing one local design solution, when significantly better solutions can be found by taking an entirely different approach.

could the design constraints be expressed in a standard form

We've been working at this level of abstraction without much change for

intermediate improvements that would move us in this direction

Defining design intent explicitly

Making 3d rep more robust so we dont need to babysit topology

Make it possible to branch and merge 3d geometry

---

4. [To be fair](https://www.youtube.com/watch?v=jv7jcciKB_s), the second half of the class was dedicated to designing a house in AutoCAD. I'm just salty that our rural high school had to cut classes after the community failed to pass a funding referendum. Funding our shitty football team was never a problem though. Priorities.

---