---
layout: post
title:      "Code Overhaul: Single Source of Truth"
date:       2021-02-16 22:04:40 +0000
permalink:  code_overhaul_single_source_of_truth
---


*Condensing my final project for cleaner, DRYer code*

I'm proud to announce that last Friday, I passed my final assessment for the software engineering program at Flatiron School! The past five months have really gone by in the blink of an eye. I am so grateful to my instructor Nancy, my counselor Gretchen, the Flatiron technical coaches, and the members of my cohort-- this has been an incredible experience, and one that I am excited to keep building on right away.

In this spirit, I'm going to share a little more about my journey building my final project, and my plans for further improvements to it-- it is my hope to get my theater playbill app to a point where I can publish it as a permanent resource at my site www.paperlessplayhouse.com), ideally by the end of this year.

#### First Assessment: Mayday!

In order to discuss my most recent code updates, I need to share that I had the apparently common but totally nervewracking experience of being assigned to come back for a second assessment. This was largely due to my working up til the last minute before my appointment "improving" the code, introducing some new bugs in the process. I sheepishly informed my assessor that my deletes didn't show up without a refresh any more, but everything else was worked fine- which I thought was true. However, the demo made it clear that far more of my functionality had been affected than I previously realized. After a few minutes of frantic debugging, we agreed that the best idea would be to do as much of the general technical interview as possible, but to come back the next day for another go at my app walkthrough and live coding. 24 hours seemed more than enough time to fix my code, considering it had been functional as recently as that morning. If worse came to worst, I could roll back to the last working git commit.

#### A Seemingly Simple Fix

I wanted to kick myself when, 10 minutes after my assessment would have ended had it gone normally, I finally discovered the source of my issues during the demo-- a simple `=` sign. I had followed a suggestion to change `!=` to `!==` when going through id numbers in my reducers, which is generally considered better practice. However, I had forgotten that the number being passed in was in string form in certain contexts and would therefore not be deeply equal to ANY of my ids, which were integers. a simple `parseInt( )` was enough to solve this problem. However...

#### Down the Rabbithole

Some time combing through my code more deeply revealed some other issues. The fact that so many of my CRUD actions redirected to my home page (see [my previous post](https://cailinkless.github.io/all_the_little_things_withrouter) for some discussion of making this work) had obscured several instances where my Redux state was never being accurately updated-- it just seemed as if it was because my home page included my index component, and my index component made the initial GET request to my backend API. On many actions, I was essentially just sidestepping Redux state entirely by hitting reset on the whole app.

#### Single Source of Truth

As I sifted through which actions were truly working (updating not only the backend API but also my frontend Redux store), it became clear that a lot of issues were being caused by treating my child model, Credit, as if it and its information was entirely separate from its parent model, Playbill. And eventually, I would like a version of the app where Credits could be independent of Playbills-- for instance, if you were working with the same actor multiple times, you could continue to assign the same Credit to new Playbills instead of creating a new Credit with identical info every time you needed them in a new Playbill. But in this iteration, Credits exclusively belong to their parent Playbill, and are destroyed alongside it if it is deleted. In this case, there is no reason to view a Credit as anything other than an aspect of its Playbill.

I slowly began editing all of my Credit actions and action types so that they essentially functioned as updates to their parent Playbill. This took a little bit of time as my format of separate Playbills and Credits was baked pretty deeply into my code, but every time I successfully changed an action in this way, user experience got smoother. Eventually, no trips to my root index were needed to force changes to show up immediately. For me, it was a concrete demonstration of the value of the [Single Source of Truth](https://en.wikipedia.org/wiki/Single_source_of_truth) principle.

#### Going Further: Separation of Concerns

My app's code is currently in its best shape yet, but my new solution leaves some things to be desired. To simplify things in my own mind, my process involved actually moving everything out of my Credit actions creditReducer into my Playbill actions and playbillReducer. This is not necessary for what I was trying to acheive, and could be highly chaotic if I continue to add more models to my application. We want a Single Source of Truth, but we also want [Separation of Concerns](https://en.wikipedia.org/wiki/Separation_of_concerns). I tried to move all my new work on my Credits, completely unchanged, back into their own space, but apparently imperfectly as this started yielding some new bugs again. Ultimately I decided I didn't have the time to finish this process adequately before my followup assessment, but it remains first on my list of things to do in my post-graduation improvement of this project.

#### Last but not least...

If any of my fellow cohort members are reading this, congratulations to all who have finished their assessments, and best of luck to those who still have them upcoming! It has been wonderful to learn alongside you all.




