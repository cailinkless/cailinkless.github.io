---
layout: post
title:      "It's The Little Things: withRouter"
date:       2021-02-07 21:23:46 -0500
permalink:  all_the_little_things_withrouter
---

*A Simple Tool That Streamlined My React-Redux App Navigation*


It's hard to believe that this is my fifth project for Flatiron! Five months really flew by, and in this final project week in particular felt like a real time crunch. This has been my most comprehensive project so far, building a Ruby on Rails API and a JS React / Redux frontend (while still just beginning to wrap my head around Redux, I feel sheepish to admit). When building an app like this within a week, relatively small bumps in the road can take a major chunk out of the remaining time until the submission deadline, so I was relieved to discover a relatively simple fix for an issue that had already cost me several hours around 3/4 of the way through the project.

My final project is my third and most detailed version of a web application idea that I've been iterating over in a lot of my work here-- and in fact, a major influence in my starting the software engineering program in the first place-- a virtual playbill builder for theater companies looking to save paper (or cut down on contact as things begin to open up post-pandemic). This project is a simple version of what an individual user's playbill creation dashboard might be like. 

As you might imagine, this involves a lot of work with forms for user manipulation of data. Though it was not a requirement of Flatiron's for this assignment, the concept itself requires that a user be able to perform full CRUD actions on every model in the program. In light of this I wanted the experience of using the forms to be easy to understand, as they would make up the bulk of the interaction with the app. So, I wasn't thrilled with the default result of filling out a form being a simple wipe of the form and then staying on the same page. Sure, the new playbill was there once I navigated back to the dashboard, but from a user's perspective, it felt unsatisfying, and worse, potentially confusing-- theoretically, a user might think their playbill had disappeared and create a whole new one!

It was a little thing -- and I was assured that having to navigate manually back to my home page to see the new playbill would not impact my ability to pass the project review -- but it bothered me. My ideal of arriving directly at the show page seemed a little beyond what I could get done within the timeframe at my current level-- as I learned, immediately pulling an id number that you've only just created in a post to a separate backend can get complicated-- but my compromise solution of automatically going right back to my index page (which would include the new playbill at the bottom) seemed fairly acheivable.

As it turns out, even that was easier said than done. The method suggested through both googling and asking around was to use this.props.history.push('/') in all my form submit methods. But this didn't seem to work on every form for some reason. Some time debugging in Chrome's DevTools revealed a new and frustrating fact: not all JS components even *have* a `history`. It can sometimes be passed down manually from a parent, but there was one form whose parent didn't have a `history` either. And even in cases where this strategy could work, it seemed messy.

However, as I did more research, a potential solution kept popping up: `withRouter`. I was a little hesitant to apply it since some of the documentation I found seemed complicated, but I'm glad I dug around until I found some use cases that seemed very similar to my situation, because in the end it was fast and flawless. I learned that components do not have access to `history` unless they are directly rendered by a `Route` -- and I had all my forms themselves separated out from the views in which they appeared. However, `withRouter` can grant direct access to `history` for components that wouldn't have it on their own.

There was nothing new to install, as withRouter is a feature of React Router which was already a requirement of the project. A simple implementation for wrapping a component in `withRouter` is as follows:


```
import { withRouter } from 'react-router-dom';

class Foo extends React.Component {
  // ...
}

export default withRouter(Foo);
```

I still felt a little bit of confusion at where to put it, since I was already exporting with a `connect()` to access global state on these forms, but once I figured out the best way to express it, these functions nested into each other just fine:

```
import React, { Component } from 'react'
import { addPlaybill } from '../../actions/playbills'
import { connect } from 'react-redux'
import { withRouter } from 'react-router-dom';

class PlaybillForm extends Component {

//...

export default withRouter(connect(null, { addPlaybill })(PlaybillForm))
```

And just like that, *poof!* -- every form I wanted could have access to `history`. A better experience for the user, and a big sigh of relief from me &#128517;
