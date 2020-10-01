---
layout: post
title:      "Trick 3: Toading Trix"
date:       2020-10-01 14:36:24 -0400
permalink:  trick_3_toading_trix
---


In Captain Toad Treasure Tracker, there are a few bonus levels for Super Mario 3D world. That got me thinking. Captain Toad can't use Fire Flowers, or Boomerang Flowers, or Super Bells, so what does he do with these unusable power-ups he finds? Well, he sells them online, of course! My project this time was writing out the logic for his store web app. Now Captain Toad, or anyone else wanting a ridiculously simplified storefront framework, can download my web app at [this link right here!](http://https://github.com/Directrix777/capn-toad-item-shop)

This project was...surprisingly simple. I can't really go into the details of a major issue I had, like I usually would around this part of a post, simply because there *aren't* any major issues I had to deal with. Instead, I'm gonna shake things up and just talk about the overall composition of the project. So, this post might only be a trick in that you believed it would be a coding trick. Hopefully you aren't too dissappointed. However, although Trix is not giving you a coding trick, Trix *is* explaining their Toad coding process, so the title is still acurate. With that explanation out of the way, let's get into the Toad. Er, code. Meant to say code.

I'm gonna focus on the User model's functionality, since it's the most important one. Users have a username, and password, but I also chose to include some admin routing. Each user has a boolean attribute that represents their admin status, with false being non-admin. The controllers for...everything really, handle these admin permissions using a simple helper method. I had already set up the standard `#current_user` helper, so I just established a helper method to set the admin status as an instance variable, like so: 
```
def set_admin
      @admin = current_user.admin unless current_user == nil
end
```
Now I can call this before any given action. I also created a helper that simply made it so I wouldn't have to type *this* so often:
```
def admin_only
    if !@admin
		redirect_to '/'
    end
end
```
I was using that code in quite a few actions, so it made sense to dry things up a bit.

Wait, hang on! That's the trick! Drying things up! It may seem kind of obvious, but if you notice you're using the same logic a lot, put it in its own method, so you don't have to type out the same code a bajillion and one times. I guess the notion of this Trick being a trick was a trick! There was a trick here the whole time! *And* I still got to make a pun with my blog title! Muahaha! Consider yourself tricked by Trix's tricky Trick! ...Now the word trick sounds weird XD
