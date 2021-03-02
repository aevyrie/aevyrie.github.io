+++
title = "Towards a Better CAD"
draft = false
[taxonomies] 
tags = [ "CAD" ]
+++

Modern CAD<sup>1</sup> software is amazing. What used to require manual projection techniques, teams of drafters, wooden splines, and and army of [ducks](https://www.core77.com/posts/55368/When-Splines-Were-Physical-Objects), can now be accomplished by a single trained  user on a laptop. Despite the revolution that CAD systems were, they are frustrating for power users and have remained fundamentally unchanged for the better part of three decades despite advances in software and hardware. Somehow, we've accepted that *this is how it's always been*, and have relegated ourselves to the fate of slamming our heads on the keyboard when the fillet feature explodes because someone in the next county touched a part file.

I started using 3D CAD as an intern 12 years ago, and have been using it professionally, personally, and academically ever since. I've used, in descending order of experience, Solidworks, Creo Parametric (Pro/E), Fusion 360, Siemens NX (Unigraphics), and Creo Direct. Like many people who've occupied the CAD-jockey seat, I've found myself in a [flow state](https://en.wikipedia.org/wiki/Flow_(psychology)) of intense focus that's difficult to replicate elsewhere. It's not an understatement to say that I love solid modeling. 

However, I don't love the process of fighting with modeling software when hard-to-debug topological problems manifest. I don't love how absolutely *fragile* parametric CAD, and more generally B-reps, are. I really don't love that those things have been constants for decades, with no end in sight.

This article, then, is the perspective of an experienced, frustrated CAD user. It's also aspirational. I hope to show you where we are, where we've been, and where I *hope* we're going.

---

1. Computer Aided Design. In this case, I'm specifically referring to the CAD used for mechanical design, sometimes referred to as MCAD.

---

# Where we are

Last year, I really began to question CAD. I like to think that sometime about 10 months ago, I experienced a feature-tree-breaking error for the 1,000th time. When that 999 rolled over, it dislodged something in my brain that was preventing me from acknowledging how infuriatingly exhausting it is. Why do we have to put up with this? Why can't we have nice things?

It's no coincidence that at the same time, I was really starting to get into [Rust](https://www.rust-lang.org/) to satisfy my longtime curiosity of memory managed "systems" programming languages. Learning Rust and using GitHub in a real project for the first time gave me equal parts admiration for the complexity involved in shipping real software, and frustration at the stagnation in engineering CAD. One of Rust's former slogans was "Fast, Reliable, Productive - Pick Three", the idea being that Rust lets you do do things without the tradeoffs you might normally expect in your software. You *can* have nice things.

As much as I see software engineers criticizing the state of their field, there is something to be said about their willingness to try new tools and develop processes to both prevent repeating mistakes, and automatically verify correctness. Not only that, but many of the most prominent tools and frameworks are free and open source, even in the commercial space! Despite the fact that you can draw parallels between the design process of digital and physical systems, the software tooling we use for mechanical design is downright archaic in comparison.

## A cargo cult

I find it rare that design engineers have an understanding of their digital design tools; many seem apathetic to understanding how CAD systems work. CAD is often a topic of shared misery, the *lingua franca* of despair for design engineers who might otherwise work in completely disparate industries. Yet somehow, engineers are ardent defenders of their CAD platform of choice, like victims of some perverse software-based Stockholm Syndrome.

To be clear, I don't blame those engineers apathetic to understanding their CAD tools. Software is eating the world, and everything we use teeters precipitously on mile-high towers of abstraction. No one person could ever fully understand the towers of abstraction they use every day, and developing an understanding of these CAD tools isn't easily accessible. However, as engineers, I think we have a responsibility to hold some level of technical understanding of the tools we rely on. Maybe that's an unrealistic aspiration.

I worry that without *understanding*, we become a [cargo cult](https://en.wikipedia.org/wiki/Cargo_cult), carrying out our mindless rituals so that the geometry kernel Gods may smile down upon their flock, and gift us with topologically correct behavior.

>First, thou shalt close all other running applications. Then thou shalt count to three, no more, no less. Three shall be the number thou shalt count, and the number of the counting shall be three. Four shalt thou not count, neither count thou two, excepting that thou then proceed to three. Five is right out. Once the number three, being the third number, be reached, then clicketh thou "Offset Surface" button. The topological inconsistencies, who, being naughty in My sight, shall snuff it.

Without understanding, we also lose the ability to meaningfully discuss or improve our tools. It has taken me months of research to get to the point where I feel even remotely comfortable writing about these topics. That's not due to the complexity of the topics though. There is a dearth of mid-level technical material on how CAD works. At the rudimentary level, I came across a multitude of industry-sponsored blog posts that explained CAD software in the same terms I'd use with a first-time computer user. On the other end of the spectrum I was following links from [Wikipedia's page on Boundary Representation](https://en.wikipedia.org/wiki/Boundary_representation), to MIT OpenCourseWare classes such as [Computational Geometry](https://ocw.mit.edu/courses/mechanical-engineering/2-158j-computational-geometry-spring-2003/), or reading through the STEP, IGES, and Parasolid file format specification documents.

Perhaps I need to be the change I want to see in the world, stop complaining, and start contributing; but this is not that article. Instead, I'd like to question why this material doesn't exist. My best guess? The world of hardware development has a culture of proprietary knowledge. To the single software developer reading this post: imagine web development, except every company involved in infrastructure or tooling was either (a) owned by Oracle, or (b) aspired to become the next Oracle. 

After dipping your toes into the jacuzzi that is open source learning material and community, trying to learn about CAD software systems is like diving into a wave pool of molten salt.

## The bad and the ugly

CAD is supposed to bring correctness and efficiency to our work. While it succeeds at rendering arbitrary views and sections with inhuman precision, it also results in huge amounts of complexity. While 3D representation is a challenging problem, I'm not aware of any axiom that states it must always have a poor user experience. Further, there is no reason to believe our current workflows are somehow optimal, or anything more than *good enough* software glued together through [decades of acquisitions](https://web.archive.org/web/20100130061758/http://www.ptc.com/company/history-and-acquisitions.htm).

We've developed techniques like skeleton modeling to limit the reference-spaghetti between parts, and the use of construction features to build more robust part interfaces. Unfortunately, these techniques rely on the fragile hope that all future users follow your rules, if they even know what those rules are in the first place. Why aren't these architectural choices an explicit part of our CAD structures?

Why do we still use versioning systems (PDM/PLM) that require exclusive file checkout? Why can't we branch and merge our solid models as we parallel path and converge on solutions? Software engineers figured out [distributed source control](https://en.wikipedia.org/wiki/List_of_version-control_software#Distributed_model) almost three decades ago.

Why aren't design constraint verifications a standard procedure for the part release process, similar to unit and integration tests in software CI? Why isn't design intent *explicitly modeled*, instead of added as fragile hacks in the feature tree? Parametric history-based modeling with B-reps seems like [accidental complexity](https://en.wikipedia.org/wiki/No_Silver_Bullet) to me. Does parametric feature-based modeling *really* provide enough benefit to offset the insane reference complexity it adds? How often could your parametric changes be made with direct modeling edits without the fragile reference-hell that parametric modeling brings?

Why is the mouse still our primary input method? Drawing is a powerful method of visual communication and understanding, often the first tool we grab when we need to communicate our ideas, yet the digital drafting table is nowhere in sight.

The boring, unsatisfying, answer to these (and most) questions is likely cost and risk<sup>2</sup>. Granted, *parts* of these features exist in *some* systems, but the industry as a whole doesn't seem to be taking a critical look at *why* we use this software the way we use it. It looks a lot like the sheer momentum of historical cruft is carrying us down an inevitable and unchanging path of developing incremental improvements to proprietary applications wrapped around proprietary b-rep kernels. Even startups like OnShape<sup>3</sup> aren't taking a truly de novo approach. As much as I admire the work that has gone into building a new CAD offering, OnShape ultimately feels like someone took the status quo of CAD, shoved it in the cloud, and slapped some SaaS on it.

What if we took a step back and rethought CAD? Maybe we could take these frustrations, the lessons of the past, and steal some ideas from the software engineers to make something truly *better*.

---

2. Yes, these are hard problems, but I'd wager those are the problems that software engineers *want* to be working on. Cost and risk are (probably) the reasons most of them aren't.

3. Well, formerly a startup, PTC bought them. RIP.

---

# Where we've been

Engineering design has a very long and storied history to which I can't do justice. If it's something you're interested in, I'd highly recommend reading [The Engineering Design Revolution](http://cadhistory.net/) by David E. Weisberg, available online for free. Instead of retreading that ground, I'd like to focus on some of the key developments in the history of CAD. This is important: it gives context to the systems we use today, as well as perspective of what is needed to see any meaningful change in the future.

## Sketchpad

Possibly the most amazing technical presentation I've seen is the demonstration of Ivan Sutherland's [*Sketchpad*](https://www.youtube.com/watch?v=6orsmFndx_o
) from 1963. It's incredible for so many reasons; what I find most extraordinary is that Sutherland came up with the concepts in Sketchpad without prior art, yet they are *still* fundamental to the CAD we use 60 years later:

- Cursor input for direct geometry manipulation
- Drawing 2D primitives in a sketching environment
- Sketch constraints
- Master modeling
- 3D projections
- Solid model representation
- Orbit camera with hidden line removal

Yet this was running on a computer built in 1958, predating integrated circuits, with less than 300 bytes of memory! In the 60 years following Sutherland's thesis there were significant advancements in 3D representation and 2D drafting workflows, but you could easily make the argument that they are all derivative of Sutherland's work.

## Abstracting the drafting process

As a natural extension of hand drafting and having much lower computational requirements, 2D CAD reigned supreme until the 90's. 2D CAD is ultimately a digital extension of traditional hand drafting, adding convenience features that takes much of the tedium out of the process: easily drawing arcs and straight lines, snapping to vertices, freely changing line styles, copy/paste, undo/redo, etc. These features add convenience to the process, but didn't fundamentally change it.

Moving to 3D was the next natural step, as it trivialized the process of 3D projection. Instead of drawing a 2D view of your part, then manually projecting it to the top and right views, solid modelling allows you to directly work in 3D as your source of truth, guaranting the validity of your 3D representation. With a 3D source of truth, 2D views used in drafting can be trivially derived from this source. Solid modeling began to take off with the advent of intuitive parametric modeling environments introduced by PTC Pro/ENGINEER and further refined by SolidWorks.

## Boundary Representation

Boundary representation (b-rep) is the name of the game in 3D CAD. You can trace the usage of b-rep back to the start of 3D CAD itself. It's a natural way to describe 3D objects, in fact, mesh file formats used in non-CAD applications like games are technically a type of b-rep. A boundary representation, as the name implies, describes an object by defining its boundary in space. The difference between mesh formats used in games and solid models, is that mesh formats don't make any guarantees about solidity. A solid b-rep is [2-manifold](https://pages.mtu.edu/~shene/COURSES/cs3621/NOTES/model/manifold.html) at all points of the boundary surface. In addition, solid models generally make more use of continuous freeform surfaces instead of discrete triangular or quad facets.

These continuous freeform surfaces can be generalized as "NURBS" or Non-Uniform Rational Basis Splines. A NURBS surface is essentially a net of splines that live in 3D space. These are a generalization of Bézier curves, which you might recognize from the [Utah Teapot](https://en.wikipedia.org/wiki/Utah_teapot). NURBS make it trivial for Industrial Designers to adjust the appearance and curvature continuity of 3D surfaces using only a handful of spline handles. In addition, NURBS are convenient to represent in parametric form - which ties in nicely with how b-rep geometry is internally represented, and they can be sampled to arbitrary precision which is needed for manufacturing.

## Too Many File Formats

The *Initial Graphics Exchange Specification* (IGES) format is the oldest 3D CAD format that still sees regular use. It was created by the US Air Force to solve the problem of CAD interoperability between their subcontractors. For context, the format specification has an 80 ASCII character limit from the days of punchcards and terminals. At its inception, IGES did not have a concept of solidity: objects were a collection of freeform, conic, and planar surfaces that had no guarantees of being 2-manifold. Although seen as an "old" CAD standard, I find that its simplicity often means that models tranferred with the IGES format will often work when STEP fails. The IGES v4.0 specification [can be found on the NIST website](https://nvlpubs.nist.gov/nistpubs/Legacy/IR/nbsir88-3813.pdf).

Perhaps the most well-known format in the CAD world is the ISO 10303 *Standard for the Exchange of Product model data* (STEP). The STEP format aims to add manufacturing data, including tolerances and manufacturing methods, to the 3D file exchange format. The STEP format is absolutely gargantuan, and in my opinion, tries to do so much that it's far too complex to approach with a green field implementation. If you don't believe me, [check this out](http://www.steptools.com/stds/stp_expg/arm.html), and zoom out of the page as far as your browser will allow. The STEP specification was first released in 1994, and is one of the most common ways to transfer CAD data between engineers and vendors. It is also incredibly common for STEP files to have broken surfaces that need to be manually repaired. Because the format is owned by the ISO standards body, its specification is not free.

The Parasolid format (*.x_b, *.x_t) gets an honorable mention here as possibly the most common proprietary (non-interchange)) format, born out of the Parasolid geometry kernel. CAD packages with the Parasolid kernel can communicate in this format natively. You can find the format specification by searching for the "Parasolid XT Format Reference".

## Geometric Modeling Kernels

One of the key things to understand about the history of CAD software is the inextricable development of modeling kernels. These serve as the core library of a CAD application, and are what allow you to modify geometry while ensuring the resulting body is solid. More importantly, these kernels are able to represent intersections between parametric curved surfaces in a precise, non-faceted format. Previously (pre-1969) b-reps were represented with faceted meshes, and as such were lossy approximations of the intended geometry. A good analogy is the difference between a bitmap image of a circle, and a vector circle. A bitmap image has a fixed resolution, and operations such as scaling will reveal that it is not a precise representation of a circle, but rather a static grid of squares. Conversely, a vector circle can effectively be scaled up infinitely without appearing blocky because it is mathematically defined and can be resampled arbitrarily.

A b-rep geometry kernel is complex and fraught with edge cases. For this reason, it's much less costly and time consuming to license an existing battle-tested modeling kernel, than build one from scratch. As a result, it's common for CAD packages from different vendors to use the same modeling kernel. The [wikipedia page](https://en.wikipedia.org/wiki/Geometric_modeling_kernel) on the topic does a great job of covering the kernels used in mainstream CAD software. To summarize the incestuous web of modeling kernel history:

* CATIA (created by Dassault Systèmes) uses the CGM kernel also developed by Dassault, this superceded their ACIS kernel.
* SolidWorks (acquired by Dassault Systèmes in 1997) uses the Parasolid kernel, licensed from Siemens. Dassault is effectively licensing a competitor's kernel due to this acquisition. Back around 2012 there was a lot of hysterics around Dassault replacing Solidworks' kernel with their own, CGM.
* Siemens NX uses Parasolid, unsurprisingly. Siemens PLM came from the acquisition of Unigraphics.
* Solid Edge (Siemens) formerly used ACIS (from Dassault)
* Autodesk's Inventor and Fusion 360 use their own ShapeManager kernel, which was forked from ACIS in 2001.
* PTC Creo products have a particularly confusing history. Their direct modeling package known as Creo Elements/Direct came from an acquisition of CoCreate, which was initially developed by HP. Creo Parametric was previously Pro/Engineer, which was merged with Elements/Direct to become Wildfire, and finally just "Creo".
* OnShape, a newcomer, licenses Parasolid. OnShape was recently bought by PTC.
* All of these kernels can (to my knowledge) trace their heritage through ACIS and Parasolid to the first generation professional modeling kernel, Romulus, released in 1978. In turn, this can be traced back to BUILD, created in 1969 as part of Ian Braid's PhD thesis.

## 

# Where I hope we're headed

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