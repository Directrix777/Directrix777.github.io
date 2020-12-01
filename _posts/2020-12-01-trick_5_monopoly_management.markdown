---
layout: post
title:      "Trick 5: Monopoly Management"
date:       2020-12-01 20:28:47 +0000
permalink:  trick_5_monopoly_management
---


For my final project, I decided to make a clone of the game Monopoly. I ended up not being able to make it what I had wanted by the deadline, but getting it to do what it does now was a lot of work. It's available for download [here.](https://github.com/Directrix777/fomopoly) Getting those spaces to render on the board required manually entering their stats into the database, which took about 2 hours. From there, I had to style each space so that it would go to the right place. Overall, css consumed the most time out of my entire project, aside from one small exception.

The main issue I struggled with was getting the users to update their location. The reducer was working fine when I was returning the initial state, but kept breaking when I tried to mutate it. I was completely baffled. Somehow my return value was breaking my code?? It seemed ridiculous! I was checking and double checking, making sure I was mapping the state correctly, but to no avail. The users would update their locations for a split second, but then it would somehow get undone.

After what felt like hours, pulling at every single thread I could find, I still had nothing. I had discovered that something was adding the old value back to the user, but I didn't know what, or why. And looking online wasn't much help either. No one seemed to be having the same issue as I was. I double-checked, just to be sure, that the reducer was the only place where I was setting the user's current location, and there it was: `if(user.current_location = 40)` I had fallen victim to one of the oldest tricks in the book. Conditionals need more than one equals!!!!!!!

But that's not my trick. It's important to look through your code for syntax errors, but you probably already knew that. My trick takes a step back a bit. Debugging can be frustrating, especially when everything seems to be absolutely perfect. I was honestly losing my mind a bit during my experience. It's important to note: The component that conditional was in? That was one that had a `mapStateToProps` function. If you're doing something that manipulates the redux store, keep in mind that it'll affect literally everything that is mapping that store to it's props, and you won't encounter the same issues I did. I didn't even know where the error was coming from. 

So that's my trick! Always keep in mind any chain reactions you might be setting off when you change something!

(P.S. Although this was my last project, this is not my last Trick! My next Trick will be my last one though.)


