---
layout: post
title:      "The Flower Pot Key"
date:       2020-11-01 23:50:03 +0000
permalink:  the_flower_pot_key
---

*The one huge vulnerability I almost left in my accounts system.*

At the risk of contributing to the ever-growing “holiday creep,” this project truly reminded me of the value of making a list and checking it twice.

My project is a Sinatra-based web application prototype for the service I am building, Paperless Playhouse(1). It allows registered users to create, save, view, and update (or delete) digital programs listing credits for theatrical productions. While not yet quite as attractive as I would want each program to look, I felt pretty happy about the basic functioning of the app as I was about to turn it in.

However, going through the spec checklist revealed a near-fatal flaw. My imaginary user “Dandy Lion” had a neat little list of programs they’d created, but when I clicked one to view it, and I was taken to a completely different program, with another title, by another user. What was going on?

A closer look at my programs.erb file revealed the issue. My code for showing each program listing was as follows:

```
    <% @programs.each_with_index do |program, i|%>
        <h2><%= (i + 1) %>. <%= program.title %></h2>
        <h3><%= program.dates %></h3>
        <a href="/programs/<%= (i + 1) %>">See This Program Draft</a>
    <% end %>
```

For those who haven’t spotted it already, the problem was an inapplicable use of my i variable from the #each_with_index method. This code assumed that the program’s id number in the database would be the same as its index number in a ruby array (plus 1). This would be all well and good if the app was tracking only one persons show programs, but the array @programs is a curated list of only programs by the current user.

I think the reason I didn’t catch this earlier was that in the beginning, I was only working with one imaginary user— I wanted to make sure all the basic CRUD functions were in working order before focusing too much on user verification(2).  But all in all, it seemed a silly mistake, easily fixed by switching that one line of code to:

```
<a href="/programs/<%= program.id %>">See This Program Draft</a>
```
        		
But wait! This way leaves it open for any user to view any program. Though their account will only show them programs they’ve created in their list of all programs, nothing stops them from viewing any program they wish if they know its url. It’s the equivalent of locking the front door but leaving a spare key blatantly sticking out of a flower pot.

Of course, eventually, the full application would allow a program to be publicly available for the duration of a play’s run, but since this piece is focused on creating and tweaking drafts, this is highly undesirable— we don’t want anyone to be able to peek at material that the creator does not consider ready yet.

Though I was taken aback that such a large flaw made it this far into my code, the good news was that it had a relatively simple fix— in fact, only one line of code needed to be changed. Originally, I had tried to protect the secrecy of a program draft by using:

```
<% if User.is_logged_in?(session) %>
```

—which is all well and good for just making sure a user is logged in, but after that, does nothing to shield the content from being seen by any user as long as they have an account to log in to— this code does not care whether the user is logged in to the same account that created the show program, just that they are logged in to ANY account at all.

The new code:

```
<% if User.is_logged_in?(session) && User.current_user(session).id == @program.user_id %>
```

—takes the necessary extra step of making sure that the current_user’s id number is the same as the id number of the user that the program belongs_to.

For anyone attempting a similar system— mind your metaphorical “flower pot keys”!

### Other Miscellaneous Observations:

* I was definitely underutilizing my #current_user method in a lot of other areas as well at first. There were a lot of GET or POST routes where I found myself trying to force an @user= for use in the view, even when it was unwieldy to do so. Switching over to #current_user streamlined my program a lot.
* When I did a lesson lab with bcrypt, it seemed like starring out the letters of the password happened automatically— but it seems not. One of my short term goals for improving this project is to black out the password input in some way.
* On the note of things I want to tweak, I’m not sure it’s correct that all my routes go directly to an erb :(view)— it seems some of these should be changed to redirects.

### Footnotes
1. A non-automated version is already up at [www.paperlessplayhouse.com](http://)
2. This also led to a brief holdup when I tried to do without users entirely in the beginning— I only made a programs table to start and then was surprised and confused when I could not create any programs. A classmate (Thank you, Kayla!) clued me in that you can not create any objects of a class with a belongs_to relationship without an instance for it to belong to— which makes sense in retrospect, but caused a complete panic at the time about why .create wasn’t working!

