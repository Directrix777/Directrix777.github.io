---
layout: post
title:      "Trick 4: Async Anomolies"
date:       2020-11-02 01:10:44 +0000
permalink:  trick_4_async_anomolies
---


This was the first project that was actually slightly tricky. My project is a simple engine that may become a game in the future. It's called "Within Our Midst: Starbase Ultima", and I'm planning on moving it to heroku once it's more developed. Expect updates!~ On a side note, this game has absolutely, positively,  ***NOTHING*** in common with the popular game "Among Us", created by InnerSloth Studios. I definitely had ***NOT*** been getting into the game around when this project started, and definitely did ***NOT*** go into this with the idea of creating a super mega Among Us map, with every task, and every room, and with cool sabotage mechanics, or ***ANYTHING*** like that, so you can't sue me, InnerSloth!

Anyways, what made this project so tricky was all the fetching that was involved. Fetches are asynchronous, and that does some weird stuff with spacetime. For example, I was trying to create an instance of a task, and give it an association based on the ID of the user I was assigning it to in the database. But for some reason, my tasks weren't getting User IDs, even though I was assigning the user these tasks after the fetch. Let me show you what I mean with some example code.
```
//**index.js**//
currentUser = new User(name)
currentUser.assignTasks(6) 
//I personally like games with 6 tasks. However, the function is dynamic so it can create as many tasks as the host wants
//I'm planning on eventually allowing a host to pick a ruleset. Now why does that sound familiar?

//**user.js**//
class User
{
    constructor(name)
    {
		this.name = name
		this.putSelfIntoDatabase()
		this.tasks = []
    }
		
	putSelfIntoDatabase()
	{
	let options = {
	//setting up create request here
	}
	fetch(*database url*, options)
	.then(r => r.json)
	.then(r => {this.setMoreVariables(r['id'])})
	}
		
	setMoreVariables(id)
	{
	this.id = id
	}
		
	assignTasks(num)
	{
	    while(*I don't have [num] tasks yet*)
	    {
		    aNewTask = new Task(this.id)
		    if(*the new task doesn't share a name with any of my existing tasks*)
		    {
		        aNewTask.putSelfIntoDatabase
		        this.tasks.push(aNewTask)
		    }
		}
	}
}
//As a tl;dr of the User class, I create the instance, make the database version with a fetch, and then set more instance variables based on the instance I just created. Assign tasks gives te user [num] unique tasks, and puts those tasks into the database

//**task.js**//
class Task
{
    constructor(userId)
		{
		    this.userId = userId
		    this.name = *Randomly generated from a list of tasks*
		}
		
		putSelfIntoDatabase
		{
		    let options = {
				//setting up create request here
			}
			fetch(*database url*, options)
			.then(r => {this.id = r['id']})
		}
}
//So I create the instance, and throw it into the database with this new association.
```


So that was a lot of code. I get it if you just read the tl;dr's, but it's all necessary for the explanation. What if, when we made the user in the database, we returned the promise, instead of just letting it disappear into the aether?
```
return fetch(putting the user into the database)
```
Now, in index.js, we can say
```
user = new User(name)
user.putSelfIntoDatabase.then(console.log(user))
user.assignTasks(6)

*some hundred lines of code here*

console.log(user)
```
So, see those 2 console logs? Which one happens first?

The one inside the fetch definitely looks like it should happen first. Those some hudred lines in the middle must take a while, even for the computer, right? And so, even though the fetch does take some time, on a processing scale, it *must* be done by the time we reach that second log! 

However! Computers are slippery creatures! They see these fetches as *paiiiinfully* slow.  They could run *thousands* of lines of code, and still finish before that fetch is done. Not only that, but the console log? That is also asynchronous! And it's *juuuust* asynchronous enough that it goes back in time, and somehow reports the user as having an ID BEFORE it was assigned, as would be indicated by the fetch finishing. The log completely misses the part where the tasks were assigned, and only walks in once the User has gotten to the point where they make no sense! Like, you *clearly* have an ID! I can see it *right there!* So why are these tasks unassociated? Red sus!

Here's what the log missed. When the User pushed in the tasks, it STILL didn't have an ID, and so, when the tasks objects were created, they had nothing to assign their userId property to. The user gained its ID, and the tasks were ready to put themselves in the database, but since the tasks were INSTANTIATED without a userId, the corresponding database entries end up not knowing which user they belong to! Then the log came in to do a task, and saw the Task objects, with the User standing over them, and called an emergency meeting. Halfway through discussion, the initial "create user" fetch, *finally* shows up, and tells us what we already knew. This was a pretty big issue, especially with that console log somehow reporting the same thing as the fetch. It took me *ages* to finally figure out what was going on.

Luckily, figuring out what was going on was the hard part. You remember that console log? The one that the fetch was logging after it was complete? We can just assign the user's tasks in there, like so!
```
user.putSelfIntoDatabase().then(r => {user.assignTasks(6)})
```

So that's my trick! If you want to be sure something only happens *after* a fetch is done, make it a callback of a then. 

(P.S. Interestingly enough, the console log we had at the bottom of the file will log the User with their tasks *before* they've been saved in the database, so the task's IDs they would get from the database will be null, even though their user IDs will have been set. Those async functions are crafty, I'm tellin ya!)
