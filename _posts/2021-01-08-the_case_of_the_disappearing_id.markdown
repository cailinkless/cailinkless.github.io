---
layout: post
title:      "The Case of the Disappearing ID"
date:       2021-01-08 20:57:05 -0500
permalink:  the_case_of_the_disappearing_id
---

*Important differences between front end JS and backend Ruby Objects*


I had learned a little JavaScript before starting my software engineering program, but it had been a struggle. Comparitively, programming in Ruby seemed as natural as breathing to me, and I dreaded the approaching JavaScript section of my coursework. To my surprise, JavaScript had evolved to be a lot more Ruby-like in the time since my initial foray into the language. This time, I felt a flow, and imagined that my hybrid Ruby-backend / JavaScript backend would come together seamlessly.

This was almost true. However, the newfound similarity between each language led me to forget a few important differences between them, especially when it came to "objects."

My project was a simple fortune-telling card app. Upon arrival, a user can draw 3 cards (a "vignette") from the 36 card "Lenormand" deck, receive some advice on how to read them together, and submit the interpretation they come up with based on their context. Since other users can later browse the vignettes and add alternate interpretations of their own, I needed to create two new objects (a vignette and an interpretation) and save them to the database with a has_many / belongs_to relationship.

Without Rails' form_for and fields_for tools to use in my JavaScript frontend, I was having some trouble creating a nested form. After tweaking and tweaking with little effect, I eventually side-stepped the problem by sneaking two forms into one. Since a new vignette object only requires the 3 cards which are already on the page and no user input, I snuck in a button labeled "Add Your Interpretation" that pulled a double duty-- it submitted a hidden fields form to create (or find) a vignette using the card info on the page, and then immediately rendered a form for an interpretation below, which had a hidden field of it's own referencing the id of the vignette just created. Only after the interpretation form was submitted did the user get taken to the newly created vignette's Show Page.

I thought this was a neat little trick myself, but as soon as I actually tried it out in the browser, I started getting errors. No matter what I tweaked, I simply could not get the vignette I'd created to show up at the end. Looking at my API in the broswer helped point me in the right direction-- it did in fact list all of the vignettes and all of the interpretations I'd created, but though the interpretations seemed to know they belonged to a vignette, the vignettes seemed to know nothing about the interpretations they'd been assigned. So what was happening?
