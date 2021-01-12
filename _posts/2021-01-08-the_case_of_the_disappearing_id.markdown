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

Some debugging (Read: A LOT of debugging) eventually led me to discover that the vignette I was working with had no ID. But how was this possible? I was able to pull up my backend database on the Rails server and look at every new vignette object I had created through the front end, and they all definitely had an ID. In fact, it would be basically impossible to create one that *didn't* have an ID.

But as it turns out, I wasn't really looking at that object I had created in my database. In a holdover from my attempt to make a very Rails-like nested form, my code in the later part of the process was still only working with the "ingredients" I had given my 'POST' fetch to build that object-- and since that object hadn't been created in my Rails API yet, those data ingredients obviously didn't include an ID. 

This is where it becomes very important to a.) remember what's actually going on with your various 'GET' and 'POST' requests to a server, and b.) to get comfortable utilizing Object Oriented JavaScript, even though the language lends itself to focusing more on functions. The fix needed was actually pretty simple-- I just needed to 'GET' that vignette I had created back from the database, and then - importantly! - wait until the appropriate time to make it into a JavaScript object to work with further.

Here are the steps as they looked in my code: 

```
function findOrCreateVignette(e) {
    e.preventDefault()
    fetch(BASE_URL + '/vignettes')
    .then(res => res.json())
    .then( vignettes => {
        if (vignettes.find(vignette => vignette.title == e.target.querySelector('#title').value)) {
            displayInterpretationForm(vignettes.find(vignette => vignette.title == e.target.querySelector('#title').value))
```

This initial step made a fetch request to find out whether there was already a vignette like this in the database-- no need to create a duplicate if all I need to do is add a new interpretation! If one is found, I go straight to adding the interpretation form. If not, I need to create a new one like this:

```						
        } else {
            let vignette = {
                title: e.target.querySelector('#title').value,
                first_card: e.target.querySelector('#first_card').value,
                second_card: e.target.querySelector('#second_card').value,
                third_card: e.target.querySelector('#third_card').value,
                first_pairing: e.target.querySelector('#first_pairing').value,
                second_pairing: e.target.querySelector('#second_pairing').value
            }
            let configObject = {
                method: 'POST',
                body: JSON.stringify(vignette),
                headers: {
                    'Content-Type': 'application/json',
                    'Accept': 'application/json'
                }
            }
            fetch(BASE_URL + '/vignettes', configObject)
            .then(res => res.json())
            .then(vignette => {
                displayInterpretationForm(vignette)
            })
        }
    })
}
```

This creates a vignette object to send to the database and then 'POST's it via 'fetch'. This is part of where I got hung up-- I was still using this vignette object at one point to try to Show Vignette at the end of the process, but my Show Vignette method works by using an ID-- and if you look at the top of the snippet, it's plain to see this doesn't have one yet!

I'll skip the displayInterpretationForm code for brevity, as it just renders the HTML and event listener necessary for the next step:

```
function submitInterpretation(e) {
    e.preventDefault()
    let interpretation = {
        vignette_id: e.target.querySelector('#vignette').value,
        content: e.target.querySelector('#content').value
    }
    let configObject = {
        method: 'POST',
        body: JSON.stringify(interpretation),
        headers: {
            'Content-Type': 'application/json',
            'Accept': 'application/json'
        }
    }
    fetch(BASE_URL + '/interpretations', configObject)
    .then(res => res.json())
    .then(int => showVignette(int.vignette_id))  
}
```

This function makes a fetch call to 'POST' the new interpretation and associate it with its vignette, but then, I don't actually want to show the interpretation- I want to show the vignette, with that new interpretation attached. So, I need to grab just the interpretation's vignette_id to pass to the showVignette function here:

```
function showVignette(id) {
    fetch(BASE_URL + `/vignettes/${id}`)
    .then(res => res.json())
    .then(data => {
        let vignette = new Vignette(data.id, data.title, data.first_card, data.second_card, data.third_card, data.first_pairing, data.second_pairing, data.interpretations)
        vignette.renderVignette()
    })
}
```

Notice (because I didn't!) that it's only here that the new vignette needs to be made into a JavaScript object again. Originally, I did this right at the end of my submitInterpretation function, which was redundant and caused errors.

Being used to working with a fully Rails-based MVC application, it was easy to forget the ins and outs of when making a JavaScript object is necessary and/or desirable. But the good news is, once I got my head around it, it didn't feel much more complicated than simply passing variables to and from views in Rails. But it's important to keep in mind that the underlying process is not exactly the same, even if the user experience will be similar.
