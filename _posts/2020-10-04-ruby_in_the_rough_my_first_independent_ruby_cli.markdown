---
layout: post
title:      "Ruby in the Rough: My first independent Ruby CLI"
date:       2020-10-05 01:36:11 +0000
permalink:  ruby_in_the_rough_my_first_independent_ruby_cli
---

Maybe you've seen this meme before:

![The Three Hardest Things to Say: 1.) I was wrong. 2.) I need help. 3.) Worcestershire Sauce.](https://i2.wp.com/onpasture.com/wp-content/uploads/2019/09/The-Three-Hardest-Things-to-Say-565x565.jpg?resize=565%2C565)

I agree with all three items on this list, but most relevant to the story of my first independent project for the Flatiron Software Engineering program is number 2 (mixed with a healthy dose of number 1 along the way. Number 3 was irrelevant as my project did not involve worstesh— westersh— winchester— condiments of any kind).

In the relative safety of earlier weeks with my project safely distant in the horizon, it was easy to think I would simply breeze through it effortlessly. I even had an imaginary version of the project that I would sometimes write a few notes or pseudocode for, so obviously I couldn’t fall behind. If anything, I figured, I was ahead!

However, as the due date grew closer, I grew increasingly overwhelmed by the idea of stitching all the different aspects of a complete program together, a program that would be different than I had envisioned as I learned that my initial idea for a planetary hour clock & calendar didn’t quite mesh with the specific goals of this assignment (I won’t waste time on the details of this right now since it’s not impossible that I will still use it for a future project!). Stress from work and home obligations also took their toll, until I found myself a few days away from the due date with my project file still almost completely devoid of code.

My original idea (second original idea?) was designed to remind myself to keep the simple parameters of the project— getting data from a remote source, processing it, and returning it in a useful format— at the top of my mind, rather than getting distracted by other features that may be required by a million different possible use cases. I did this by choosing to build an application with a decidedly impossible use case— the Superhero Recruiter.

The idea was to use a superhero API I found to offer a curated list of superheroes (and villains) that one could hire for achieving personal missions and schemes. The API included every characters alignment of good or evil, so I first requested to know if my user’s mission was benevolent or nefarious— the former would limit the list to only heroes and the latter to only villains. From there, the user would select their most-needed super skill — e.g., speed, strength, intelligence, etc. — and the recruiter would return a list of superhuman that had a 90% or above according the the “powerstats” attribute in the API.

I enjoyed the idea (not least the overdramatic language I could use in the interface), but I pretty quickly ran into problems with the API, I think having to do with my access token. I definitely would like to troubleshoot this issue a little more and try again sometime, but now in the last stretch of project week, I needed a new solution fast so I could focus on my code.

A makeup API I had bookmarked in the early stages of my project came to my rescue. I noticed that some of the tag options for makeup in the database involved sorting based on certain ethical values — natural, vegan, fair trade. Using these to replace my initial narrowing question of “good vs. evil”, I realized that the logic I had started building on would still hold up perfectly for my new project— the Ethical Makeup Finder.

I continued off of the menu building part of the process, at first largely because I enjoyed it and found it the easier task than writing the code to actually process my information. However, midway through a strategic advantage started to reveal itself as well— mapping out the entire flow of the use of the CLI and using “puts” messages explaining the intended results as placeholders actually clarified exactly what code I would need to write and where in the flow I needed to go— the steps for getting information from the API and what to do with it suddenly seemed very obvious. Once the menu worked perfectly, I figured, everything else would be easy and done in a snap.

However, there were definitely still some pitfalls awaiting. My menu code definitely fell pray to a few infinite loops where I’d made some awkward applications of “if” and “while” statements. And in the object orientation aspect of things, I was definitely losing track of where I was with certain methods (“What is my SELF now???”).

But the biggest hurdle was just how long I had put off asking for help. I’d been avoiding it for fear of exposing how stuck and behind I felt. Reaching out to my educational advisor in a panic, I asked what would happen if I missed my first project deadline— and learned that I was actually one of many who felt nowhere close to finishing. She outlined some possibilities of what could happen, but pointed out that I had not been reaching out to the community much (that week, barely at all). Though on a Saturday it was too late to jump in on an official study group, there were still plenty of my fellow students to work with, for advice or even just for moral support. 

This ended up being the key that unlocked the rest of the project for me. In a Zoom meeting with fellow students, I saw firsthand that I was far from the only one to fall down confusing rabbit holes with “if” statements and “while” loops. Or to change my project several times before settling on one two days out. Even after the group left the Zoom room, we kept in touch, and tracking each other’s progress and sending questions and code snippets back and forth. And as I hit a wall right at the end— when it seemed like all my “perfect” CLI commands and “perfect” API code just wouldn’t fuse together somehow, a classmate who had already finished her project hopped on a screen share to be a second pair of eyes (I’d lost my SELF again!).

My key takeaways from this project week:

—> Build the logic out of how your program will work as much as possible with simple input & output, even if you’re not sure exactly how you need to operate on all the information in the backend yet.

—> Pry will save your life. And help you find your SELF ;-)

—> Community will save your life. Community will save your life even more than Pry will save your life. Find one, leave the coding hermit cave and never look back.

—> If you’re using a “while” to control for incorrect user inputs— make sure you always  ask for input again within the while loop OR YOU WILL FALL DOWN AN ENDLESS RABBIT HOLE
OR YOU WILL FALL DOWN AN ENDLESS RABBIT HOLE
OR YOU WILL FALL DOWN AN ENDLESS RABBIT HOLE
OR YOU WILL FALL DOWN AN ENDLESS RABBIT HOLE
OR YOU WILL FALL DOWN AN ENDLESS RABBIT HOLE
OR YOU WILL FALL DOWN AN ENDLESS RABBIT HOLE
OR YOU WILL FALL DOWN AN ENDLESS RABBIT HOLE
OR YOU WILL FALL DOWN AN ENDLESS RABBIT HOLE

control+c

—> Every “if” does not need an “else.” I’m not sure if this was obvious to everyone else in the world, but not me, and I’ve written some truly odd code in the past simply because I was trying to force “else’s” I didn’t need.

There’s more I might like to do on this app if I were to build it out further, but I’m pretty happy with it as a minimum viable product. And maybe some day when I have some free time on my hands, I can still grace the world with my superhero recruitment app :-)
