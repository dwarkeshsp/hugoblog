---
title: "Book Review - Unix: A History and a Memoir"
date: 2021-06-16
draft: false
---

_Buy [UNIX: A History and a Memoir](https://www.amazon.com/gp/product/B07ZQHX3R1) by Brian Kernighan_

Watch _Brian Kernighan [interview](https://www.youtube.com/watch?v=EY6q5dv_B-o) Ken Thompson_

## I. Creation

The story of the creation of Unix is kind of insane. In 1964, MIT developed CTSS, an operating system that allowed multiple users to use the same machine at the same time by quickly alternating between the users' tasks. It was a great success, and MIT worked with Bell Labs and General Electric to develop a follow up called Multics. Among the researchers from Bell Labs working on Multics were Ken Thompson and Dennis Ritchie. In this book, Brian Kernighan doesn't go into too much detail about why Multics failed except to say that it suffered from the second system effect, where engineers respond to a successful product by creating an overcomplicated second version which tries to do too many things. In 1968, Bell Labs pulled out of the Multics project.

It was around this time that Ken Thompson found a little used PDP-7 workstation (which was apparently a shitty computer even by 1969 standards). At first he built a space travel game on the machine. He then decided to write a disk scheduling algorithm on it, but he couldn't test it without writing some other programs to load the disk with data. Kernighan writes, quoting Thompson:

> “At some point I realized that I was three weeks from an operating system.” He needed to write three programs, one per week: an editor, so he could create code; an assembler, to turn the code into machine language that could run on the PDP-7; and a “kernel overlay—call it an operating system.”
>
> Right at that time, Ken’s wife went on a three-week vacation to take their one-year-old son to visit Ken’s parents in California, and Ken had three weeks to work undisturbed.[^1]

[^1]: And here I'm left thinking about the three weeks I've spent since my spring semester ended doing close to nothing.

A few people at Bell Labs realized that this Unix thing could be really useful, and they asked the company for the more powerful PDP-11 to do further development. But Bell Labs was so traumatized from its experience with Multics that they wanted nothing whatsoever to do with operating systems and rejected their proposal.

The Unix group then told management that they wanted a PDP-11 in order to help the Patent department prepare applications, not to write operating systems, no sir! In the daytime, the patent people used the machine, and at night, Ken Thompson, Dennis Ritchie, Brian Kernighan, and other were writing software on it. Eventually, the Patent people decided that they wanted the machine to themselves and bought the Unix people their own PDP-11.

Thompson wanted to eventually write Unix in a programming language above assembly, but the existing languages like Fortran and Cobol were too big to write operating systems on for computers with 8 KB of memory. He developed a language called B, but it didn't have the abstractions necessary to make an operating systems.

Dennis Ritchie then developed C, which was typed and which formally separated pointers and integers. You could get the number of bytes you wanted from memory based on the type of your variable, and you could do things like add one to a pointer to get the next item in the array. But even this wasn't enough to develop an operating system, so Ritchie eventually added structs, and at this point Unix was ready to be written in C.

Thompson, Ritchie, and others started presenting Unix and C at conferences and at colleges. Bell Labs would give universities the source code for Unix basically for free and to companies for a steep commercial fee. But since there was no support or documentation yet, Unix was hard for outsiders to understand or adopt. Eventually, someone wrote a line-by-line commentary of the source code - this book was technically a trade secret but was copied widely.

Researchers at Bell Labs were making all sorts of tools and languages on Unix, like Yacc, Lex, and Make, not to mention the document preparation programs necessary at a big company publishing all kinds of papers and patents. Unix started being ported onto to more and more systems, and by the 80s, it dominated the operating systems market.

Around this time, AT&T was getting broken up by the government, and it was losing patience for research that was unprofitable and unrelated to its main telecommunications business. Eventually, the management at Bell Labs realized, "Hey, this computer and operating system business might actually take off. Maybe we should try to make some money off it instead of handing out our source code willy-nilly." AT&T tried to trademark the UNIX name and sue Berkley for developing an operating system derived from Unix. But by then, the cat was already out of the bag. Kernighan writes:

> In retrospect, I suppose one could say that all the legal wrangling was a by-product of AT&T’s early and almost accidental decision to make Unix available to universities. As Unix spread from universities using it for free to companies that were willing to pay for it, it became commercially viable, at least potentially. But it was too late for effective protection. Even if AT&T’s source code was restricted, the system call interface was in effect in the public domain, and there was so much expertise in the community that creating versions unencumbered by AT&T licenses was almost routine. The same was true of application software like compilers, editors, and all the tools. To mix several metaphors, AT&T was trying to lock the barn door after the crown jewels had flown the coop.

A thousand flowers continued to bloom, but none blossomed as well as Linux, which started as a hobby project of a 21 year old Finnish college student named Linus Torvalds. The rest, as they say, is history.

## II. Philosophy

In 1991, Richard Gabriel published an [article](https://dreamsongs.com/RiseOfWorseIsBetter.html) where he explained that Unix and C were so successful because they followed the "Worse is Better" philosophy:

> Both early Unix and C compilers had simple structures, are easy to port, require few machine resources to run, and provide about 50%-80% of what you want from an operating system and programming language.
>
> Half the computers that exist at any point are worse than median (smaller or slower). Unix and C work fine on them. The worse-is-better philosophy means that implementation simplicity has highest priority, which means Unix and C are easy to port on such machines. Therefore, one expects that if the 50% functionality Unix and C support is satisfactory, they will start to appear everywhere. And they have, haven’t they?
>
> Unix and C are the ultimate computer viruses.

This explanation helps us understand why Unix was successful and Multics was not. Naively, you would have expected Multics to succeed. After all, whereas Multics was developed by dozens of researchers across MIT, Bell Labs, and General Electric, Unix was created by a single person looking for something to do while his wife was out of town.

But maybe Unix succeeded because it was initially developed in a three week frenzy by one person. Ken Thompson was trying to test out a disk scheduling algorithm on a shitty machine with low memory. He didn't have the time, inclination, or pressure to complicate things. He built a simple recursive file system, he allowed any program to process any kind of file regardless of format, and he implemented a small number of intuitive system calls. At the time, he was doing what was expedient, but only later would the wisdom of these decisions become apparent.

Kernighan writes:

> The hierarchical file system was a major simplification of existing practice, though in hindsight it seems utterly obvious—what else would you want? Rather than different types of files with properties managed by the operating system, and arbitrary limits on the depth to which files could be nested in directories, the Unix file system provides a straightforward view ...
>
> Files contain uninterpreted bytes; the system itself does not care what the bytes are, or know anything about their meaning.
>
> Files are created, read, written and removed with half a dozen straightforward system calls. A handful of bits define access controls that are adequate for most purposes.

If Thompson were part of a big research project, he would have been asked to add additional features which would complicate his system and thus render it unable to run on his shitty machine. More importantly, Unix would have become inaccessible to possible adopters trying to understand how it works. They wouldn't be able to write the useful programs on it which would attract even more adopters.

The story of C is very similar. As Linus Torvalds explains in the video below, you can practically see the assembly that a C program will generate. In 1972, this lack of overhead was a necessity for any programming language being used to write an operating system. Today, it is a boon to programmers who want to know exactly how their program will work at the level of individual instructions and bytes of memory.

{{< youtube CYvJPra7Ebk >}}

If you want to build a program that runs on thousands of computers, you had better assemble a big team of expert engineers. But if you want to build a program that runs of millions of computers, well, find the smartest person in the room, give him the shittiest of those million computers, tell him what you want, and leave him the hell alone.

## III. The adjacent possible

It's funny how often Kernighan says that some feature of Unix was utterly obvious in retrospect but new and innovative at the time. This makes me wonder - if the ideas behind Unix were so obvious, was someone bound to organize them into an operating system even if Ken Thompson and Dennis Ritchie had never existed? Was someone in the late 60s/ early 70s bound to create a portable operating system written in a programming language above assembly that had a hierarchical file system, pipes, simple system calls, and the rest?

In _How Innovation Works_, Matt Ridley argues that innovation is inexorable - that if someone had run over Thomas Edison the day before he found the right filament for a light bulb, another inventor would have lit up our homes with only a negligible delay.

Whether the same is true for Unix is hard to tell. There were so few people who had access to a computer around 1970 that the absence of two genius programmers with experience writing operating systems could easily have stalled the development of a Unix-like operating system.

However, it would be easier to believe that the Unix group was irreplaceable if it had taken decades after the failure of Multics for someone like Ken Thompson to come around and try building another operating system. But in reality, it only took a year, and Raytheon was already developing another operating system called TENEX at the same time.

So suppose that someone would else ended up creating a successful operating system. How different would it have been from Unix? It is easy to think that the legions of operating systems developed since Unix have given us an understanding of all that is possible. But this is wrong because

1. Almost all of the operating systems since Unix have been influenced by Unix.
2. Computing hardware in the 80s and 90s was designed and evaluated based on its capacity to run Unix and C fast, and this did no favors to anyone trying to build an operating system on completely different principles.

Whether Unix was so close to the platonic ideal of operating systems that its design was inevitable will continue to be a fascinating question about the history of the computing. And unless someone builds a successful operating system that is completely different, the question is bound to remain unresolved. As for Kernighan's opinion:

> Unix was probably a singularity, a unique combination of circumstances that changed the computing world. I doubt that we will see anything like it again in operating systems, but there will surely be other occasions when a handful of talented people with good ideas and a supportive environment do change the world with their inventions.

## IV. It takes an village?

Kernighan writes:

> [T]he number of people contributing to Unix in the early days was tiny; arguably the core was a single person, Ken Thompson, who is certainly the best programmer I have ever met, and an original thinker without peer. Dennis Ritchie, whose name is linked with Ken as the co-creator of Unix, was a vital contributor, and his C programming language, central to the evolution of Unix in the early days, is still the lingua franca of computing.
>
> It’s instructive to examine the languages that programmers use every day and see how often they were originally the work of one or two people ... Google, Facebook, Amazon, Twitter, Uber, and any number of other companies that went from startups to multi-billion-dollar enterprises originated with a bright idea by one or two people.

But if all it takes is one or two brilliant people to come up with a great idea and implement its first iteration, then does this mean research institutions like Bell Labs are useless? Maybe Bell Labs was just really good at hiring scientists and engineers who were bound to do interesting things anyways but was in no way the cause of their success.

This seems improbable. No way could a single institution like Bell Labs have monopolize as much talent as their share of total 20th century innovations would imply. So what specifically was Bell Lab's contribution? Here are some possible answers:

### Agglomeration benefits

A lot of research shows that cities are big hubs of innovation, especially if they have a university and lots of companies from the same industry there. This is because cities make it really easy for smart people with expertise to interact with each other and talk about their ideas. If an idea seems particularly good, then they can try to build it out and start a new company. [^2]

[^2]: See [here](https://www.theatlantic.com/ideas/archive/2020/07/americas-innovation-engine-slowing/614320/) for more.

We can think of Bell Labs as a tiny city, making up for its small size by its dense concentration of talent. Kernighan writes:

> The size and scale of Bell Labs meant that for almost any technical area, there were multiple experts, often world leaders in their field. Furthermore the culture was strongly cooperative and helpful. It was absolutely standard procedure to walk into someone’s office and ask for help; most often the person being asked would drop everything to assist.

Reading the names mentioned in the book, it sounded like almost anybody who was anybody in computing in the 70s and 80s had done a stint at Bell Labs - Ken Thompson, Dennis Ritchie, Doug McIlrory, Bill Joy, Eric Smidt, Jon Bentley, Bjarne Stroustrup (somebody stop me before I list so many people that the concept of names doesn't make sense to me anymore).

These people would constantly have new ideas which fed off of each other. Ken Thompson makes Unix, Dennis Ritchie decides to create C so that Unix can be ported easily, Blarne Stroustrup then build an object oriented preprocessor for C which becomes C++, and so on.

At Bell Labs, ideas were not only having sex, they were total sluts.

### Freedom

Kernighan quotes the vice president of Research at Bell Labs:

> Freedom of choice is of utmost importance to the research scientist, because research is an exploration of the unknown and there are no road maps to tell what course to take. Each discovery affects the future course of research and no one can predict or schedule discovery. Thus Bell Laboratories research managers have provided the maximum possible freedom to the research staff, consistent with the institutional purpose. Research people have been chosen for their creative abilities and are encouraged to exercise these to the fullest.

If you're going to give people this much freedom, you had better make sure that you've hired the kinds of people who will make good use of it. Freedom, like the death penalty, is something that must be given judiciously and sparingly, if at all. Kernighan writes:

> One resource was very carefully managed: hiring. In 1127, we typically could only hire one or two people a year, and almost always young ones, so hiring decisions were made very cautiously, perhaps too much so. This is of course a familiar problem in university departments as well. It is often unclear whether to go after a star in a particular field, or someone else who is broadly talented; as Steve Johnson once put it, should we be hiring athletes or first basemen? My preference has been for people who are really good at what they do, without worrying too much about specifically what it is.

I wish Kernighan went into more detail about how they decided who to hire. For example, how do you know that the thing that somebody is really good at could actually lead to important research? Or do you not care if the thing they're good at matters, you only want to know that they can master a subject? I'm still not sure how Bell Labs got this right, but clearly they did.

### Promotion

Kernighan:

> It’s interesting to compare the Bell Labs evaluation process with how it’s done in research universities. In the latter, hiring and especially promotion are strongly influenced by a dozen or more letters solicited from prominent outside researchers who are in the same specialty. This tends to encourage deep expertise in narrow fields, since the goal of a reviewee is to master some field so well that external reviewers can legitimately say “This person is the best person in this sub-field at this stage of his or her career.”
>
> By contrast, Bell Labs created a rank order for every researcher, from the bottom up. Each department head ranked his or her people; those rankings were merged by department heads within a center, and those in turn by the next two levels, so by the end everyone’s approximate position in the whole population was determined.
>
> A person who did great work in a narrow field might well be ranked highly by his or her immediate management, but no one further up would likely know of the work. Interdisciplinary work, on the other hand, stood out at higher levels because more managers would have seen it. The broader the collaborations, the more managers would know about it. The end result was an organization that strongly favored collaboration and interdisciplinary research. And because the managers who made the decisions had come up through the same process, they were inclined in the same direction.

I'm not sure why researchers at Bell Labs would even care about their evaluations if they're given so much freedom that their evaluations don't really matter. Nonetheless, to the extent that the researchers did care, they were encouraged to do impactful, interdisciplinary work.

### Funding

Kernighan:

> Stable funding was a crucial factor for research. It meant that AT&T could take a long-term view and Bell Labs researchers had the freedom to explore areas that might not have a near-term payoff and perhaps never would. That’s a contrast with today’s world, in which planning often seems to look ahead only a few months, and much effort is spent on speculating about financial results for the next quarter.

Peter Thiel makes a similar point about monopolies in _Zero to One_ (though he's talking about natural monopolies, not government enforced ones like AT&T):

> Monopolists can afford to think about things other than making money; non-monopolists can’t. In perfect competition, a business is so focused on today’s margins that it can’t possibly plan for a long-term future. Only one thing can allow a business to transcend the daily brute struggle for survival: monopoly profits.

The example of Bell Labs really shake up my beliefs about the value of competition and a free market. Was it justified for AT&T to basically put a tax on every telephone call in America if they used part of that money to fund important research? Why did AT&T even bother to fund basic research if they were guaranteed a government monopoly anyways? Are we worse off for the government deregulating telecommunications and breaking up AT&T because it led to end of Bell Labs? Why haven't private firms been able to do the level of groundbreaking research that Bell Labs did? I have no answers, only questions.

Here's how Kernighan sums it up:

> The big secret to doing good research is to hire good people, make sure there are interesting things for them to work on, take a long view, and then get out of the way. It certainly wasn’t perfect, but Bell Labs research generally did this well.

---

As a programmer and a tech history nerd, I loved this book. I'm really glad that I got to hear the story of Unix from someone who actually contributed to the project and was contemporaneous with its development. I would recommend this book to anyone interested in the history of computing or in understanding how great research happens.
