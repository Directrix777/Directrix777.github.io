---
layout: post
title:      "Trick 1: Pokemove Problems"
date:       2020-08-01 20:14:53 +0000
permalink:  trick_1_pokemove_problems
---


So my first major project was creating the poke moves-cli gem. It works now, and it’s available at [this link](https://rubygems.org/gems/pokemoves-cli) for all your Pokemon move based needs! It’s a cli that tells you what moves a Pokemon can learn, as well as a specific move’s type, so you can have that sweet sweet type advantage! This also works in reverse. You can see whether or not a Pokemon can learn a specific move!

I’d say the trickiest part of this was getting the Move class to work with the cli.
See, initially, the move class had two attributes, a name, and a type. It got it’s name from the PokeAPI, which has move names stored in this format, using Baby Doll Eyes as an example: “baby-doll-eyes.” This made accepting user input very problematic. The cli was using the Move class’s #find_or_create_by_name method to avoid scraping the same move twice, so this was an issue.

At first, my way to solve this was to just chain string methods together to change the user input to match the format, as well as to output the move name with each word capitalized. But then I realized that if I ever wanted to change how that worked, I'd have to go to each occurence of the methods and change the logic. So, at first, I put the logic to beautify the name within the CLI class, but I noticed that only move names are ever passed in, so I added that logic as an attribute of the Move class instead. Now Move had the additional attribute pretty_name, and this could be called anywhere, so long as the move object was already instantialized.

Now I just had to adapt my code to reflect this new property of moves, and it was a very simple process, mostly. The Pokemon class, however, was being kind of tricky. It had a method called #get_moves, which returned an array of move names. But now that the functionality of the Move class had changed, #get_moves needed to return move *objects,*  which was problematic. When the CLI was trying to display which moves a Pokemon could learn, it couldn't becuase while it was trying to scrape each move's type from the API, to initialize the objects so it could print a numbered list of pretty_names.

My solution to this was to have each Move instance only ever set their type attribute when it's first accessed. If the attribute "type" has a value equal to nil, scrape it from the API. Whether the type was null or not, it's definitely something now, so we can return it. Simple logic, but it took me a while to puzzle out. Now everything functioned as intended. Move names were displayed as pretty_names, and they could also initialize themselves with no issue.

So that's my trick! If your object has data that needs to be scraped, scrape it on request instead of on initialization!~ 
