---
layout: post
title:      "Trick 2: Fumbling with Forms"
date:       2020-08-30 20:07:28 +0000
permalink:  trick_2_fumbling_with_forms
---


Most of this project was relatively simple for me. Especially considering the lab before it basically gave you everything you needed. However, there were a few things that weren't in the cirriculum that might be useful to others starting out their project, so I'll show you what I used to give my webpages that extra flair.

First, some context. For my project I created a web app for creating and storing Dungeons and Dragons characters, which is available for download [here](https://github.com/Directrix777/dnd-mvc). Each character had a few basic statistics, like strength, or intelligence, as well as class levels, a race, and an alignment. The class levels could be tracked with `<input type="number">`, since any given character can have more than one class. But the races and alignments would be so much more convenient if they were in a dropdown list. 

After some googling around, I found the `<select>` input type, and it's surprisingly easy to use! You create them like this:
```
<select id="var" name="var">
    <option value="value">A value</option>
	<option value="another_value">Another Value</option>
</select>
```
Putting these dropdown lists into the create form was easy, if tedious. The edit form, however, was trickier. See, I wanted these dropdown lists to have a default value, and although there were lots of pages for adding a default value for *creation*, I couldn't find anything for selecting a default value based on an existing variable, such as a character's race.

My workaround probably isn't the most efficient, but since the edit action was importing an instance of the character class anyways, I decided to just embed some ruby into each option. Here's an example pulled directly from my form:
`<option value="Elf"<%="selected" if @character.race == "Elf"%>>Elf</option>`
The embedded code just adds the selected attribute to that race if the character is already that race, which sets that option to the default. It didn't exactly look pretty, but it worked, so I could move on.

So I guess that's my trick. Just because a solution doesn't look pretty doesn't mean it isn't the right answer. If you can't come up with any other way, just do it the way that works and move on. You can always come back to it if you discover some way to make it better. 
