## 设计之理

This section includes some insight into common design pitfalls and how to avoid them. Though the rest of the document focused on the technical issues around modding, this section is devoted to game design.

There is no perfect way to make a mod. Understanding that I am unqualified to write this section I enlisted the aid of some of my fellow mod makers (and my favorite designer) so that readers could get a wide range of opinions. Each of the Danger sections are concluded by a question that has been asked to the designers about how they deal with the challenges of development. Their responses offer very practical advice for all mod makers.

I would like to thank the following people for responding to the interview questions and providing input for this section:

- **Soren Johnson** - Civilization IV Lead Designer.
- **Jon Shafer** - Civilization V Lead Designer.
- **Sevo** - Designer of Sevomod 3, Sevo’s Civilopedia and Sevo’s Faces of God.
- **Rhye** - Designer of Rhye’s Catapult, Rhye’s of Civilization and Rhye's and Fall of Civilization.

### 做出你想玩的游戏

This is the first rule: you are the only person who has to like what you have created. Don’t make the mod you think other people will want, don’t change a design based on outside feedback unless you agree with it. It is better to have an unpopular mod you enjoy than a popular one you don’t. That goes for all the advice in this section, if you don’t like it, don’t use it. This is the advantage of modding, Firaxis has to worry about making sales and mass appeal, but we don’t.

The truth is, if you stick to what you like in time you will find others that are looking for the same thing, so it will work out anyway.

### 避免设计缺陷

#### 多的危险

“Perfection is not achieved when there is nothing left to add, but when there is nothing left to take away.” -- Antoine de St. Exupery

Every new object has a cost, not just in what it takes to create, test and manage, but the player has to keep track of it as well. In general we should only add items because they offer a significant improvement to some aspect of the game, and not just to have more units, more resources, etc.

It sounds good to be able to offer a long list of new objects. That was much of the appeal of the very popular Civ3 Double your Pleasure (DyP) mod. But the success of DyP wasn’t because of all the new objects, but because each one had a distinct functional purpose. Adding buildings is easy, making them truly worthwhile is the hard part.

Is it needed? Would it be missed if it was taken out? Is it functionaly unique? If the answer to these is no, it should be considered for removal.

**Q: There always seems to be one more building or unit that would be perfect to have in the game, how do you decide what to include and what to keep out?**

Soren Johnson

----------

    Look at other successful games as an example of the number of units/buildings/choices to include. For example, Blizzard RTS's stick to a very rigid limit of 12-15 units per faction. That's a reasonable number of choices. We tried to have about 8-10 unit choices per era in Civ4, with the understanding that there would be some overlap. This is one area where there really might be a "magic formula" for how many choices are just right for fun gameplay.

Jon Shafer

----

    As a designer a lot of times you just have to go with your gut. What "too much" or "too little" means varies from person to person. I think most designers tend to lean in the direction of "what would I enjoy playing with." For example, in Civ 5 Paratroopers are in the game to start, whereas that wasn't the case in Civ 4. Why? Really, just because I thought they were cool. It's a way to spice up combat in the modern era. Once you've added or changed anything though, it's vital to play with what you've done to make sure it works. So many of the ideas I've thought were cool on paper turned out to be duds in the game. As experienced designers like to say, design is more about making something work than having cool ideas.


Sevo

-------

You need to be careful about adding every new unit that pops up or you can think of--at some point you'll lose focus. I prefer a slower staged approach: add a unit or two and play out a game; see how the new units work in the overall scheme. Actually, that's a point I should have made earlier: if you're in any way responsible for the overall direction and management of a mod, you ought to be playing it pretty regularly during "upgrading". It's the single best way to learn how your mod is functioning and what the problems are.

Rhye

----------

Statistically, I decline 80% or more of the proposals I receive from users, for different reasons. One is the design scheme: often they don't fit it. Another reason is the lack of graphics: I don't add anything that hasn't an adequate representation. Another reason is complexity / feasibility: often what they propose can't be done or is too hard: but in most cases the idea is good and a simpler variant would be fine.

And a final aspect, one always has to ask himself: will the AI know how to use this change?

#### 特点的危险

Why do games based on movies and movies based on books always seem to be bad? There are exceptions, but we are usually disappointed with the results of these conversions. The reason is that the design of the new game or movie is based so strongly on the flavor of its source that its own functional design suffers.

At some point you should look at your mod as just a functional process, a board game without any stylized components, an exercise in mathematics. A game must be enjoyable at this layer to be fun. Some games are so well designed they only exist at this layer and are still amazing to play (chess, othello, tetris, etc). But a game that has incredible flavor but no meaningful functional elements will always be a bad game.

If you fall in love with a concept that seems like a great idea, but doesn’t have any functional value, the temptation will be to come up with a functional need. There isn’t anything wrong with this, some ideas come from flavor, and some come from function. Different people are more apt to think from one end or the other. But, be aware of the danger that designing from flavor presents. If you aren’t able to come up with an elegant functional need you are best off to remove or change the flavor rather than let the functional design suffer for it.

This danger is even more prevalent when you are basing your mod on a known source. It is nice to already have all that flavor developed for you, but it can be as constraining as it is helpful. You will either have to let your design suffer (to what level is up to your own ability to find creative elegant solutions) or be willing to step outside the bounds of the source material and develop new elements, or exclude elements despite their existence in the source when the material doesn’t improve the game play of your mod.

**Q: How do you balance between Function and Flavor? What do you do when you have a functional need for an element but aren’t excited about the flavor ideas you have? How do you deal with a good flavor concept that has no functional need?**

Soren Johnson

----------

It's easy to think that Function (game play) should beat Flavor, but really the Flavor is the whole reason people decide to play a game in the first place. They come for the Flavor; they stay for the Function. You've got to have both elements, so if you have a nice Flavor bit without a matching Function, either work harder to find the Function or cut it out entirely.

Jon Shafer

-------------

Function and Flavor definitely need to be balanced. How you end up there can vary quite a bit from feature to feature. I think most of the time it's best to start with Flavor, because that's what's going to make your work memorable. It's one of the reasons why Alpha Centauri is such a beloved game. Every game starts with a "hook," where you're trying to sell people on an idea. In a broad sense, Civilization is about running an empire and crafting history in the way YOU'D like to see it. Everything has to somehow point back to that basic premise. You also have to apply that same thinking to individual mechanics. "What do I expect a player to think or feel when they play or use X?"

That having been said, there are also times as a designer you have an idea for a system that you're really excited about, and you have to craft some flavor around it. This is kind of how the Social Policies came to be in Civ 5. I had a pretty good idea of how I wanted the mechanics to work at the start of the project, but many of the names and details came later. And it went through a few variants - the system didn't pop out of my head fully-formed or anything like that. It needed some time and love before it ended up in a place that I was happy with.

Sevo

-----------

This is probably the most important question to answer in creation of a mod, in my opinion. This game is so open to creation and modification and there are so many interesting ideas floating around that one tends to want to throw it ALL together and it's easy to get carried away. I think what you leave OUT of a mod is as important as what you put in.

To that end, when I'm working on a mod, I tend to focus first on function and second on flavor, because if it's beautiful and it's unique but it's totally unplayable because of balance issues or game play, then no one will play it. That's really the trick, I suppose, to making a great mod: creating the flavor and style you picture without losing the function.

As to the need for a functional element without flavor and vice-versa: this is where the ingenuity and creativity of modding come into play. You will find things that are imbalanced, or pieces you need, or things that must be adjusted: you as a modder can implement them any way you can imagine, and your charge is to keep the feel of your mod while you do it. On the other hand, sometimes you have a flavor, an idea that you really love, but the function is poor. You have two choices: find a way to MAKE it function, or drop it despite its flavor.

For example, the settler religion mod: At one point I included this in Sevomod, but it completely unbalanced game play since ALL new cities started with a religion. I really like the idea, but it doesn't work in the mod, so I had to lose it. It hurts, but if you can't find a way to keep it functional, then it's better to leave it out. I've tried and dropped dozens of ideas/units/etc because they just didn't fit.

Rhye

--------

I always tend to add only things that have both function and flavour. I mean that if I have a cool unit designed by somebody else to add, but nowhere to put it without unbalancing the game, I won't bother. And if I feel a gap should be filled by a unit, but I don't have an adequate graphical representation, I won't add it as well.

If we speak about maps, then the function is more important than the flavour. See how many giga maps are being / have been developed: they're pointless, because nobody has a computer strong enough to play them.

#### 模式的危险

Starcraft was a big eye-opener for me, an RTS that offered 3 different forces that were balanced but completely unlike each other. It’s so much easier to balance a game by making the options mirror each other but it’s more enjoyable for the player to have a variety of options that are dissimilar.

We could be talking about any game feature. For example we could have a series of Civics that gave +3 research at the first level, then you could upgrade to one that gave +9 research when you learned the appropriate tech and up to +15 with the final tech. Or you could have a series of civics, one of which gave +3 research, another gave +3 gold and one that game +3 hammers. In either case the result may be balanced but uncreative, and dull for the player.

The obvious response to this is that if we don’t stick to patterns then invariably some options will be better than others. Better or unbalanced options are the same as no options. But I think we have some flexibility here. This is the challenge of design, presenting the player with multiple options with different risks and rewards for each, and having each viable for different reasons or in different situations.

**Q: It seems easier to build on a tested design, and have new elements be functionally similar concepts to existing ones but with a new power level or different range of effect. How do you determine when this isn’t innovative enough? Or is that even a concern for a strategy game?**

Soren Johnson

--------

Innovation should not be a primary goal - fun game play should be the goal. Innovation is the process of both improving old systems as well as creating new ones out of thin air. However, it is always best to understand what your game's aesthetics are so that your new systems aren't a mis-match. For example, Civ's aesthetics are tiles, turns, and boxes-filling-up-with-stuff.

Jon Shafer

-----------

I think it comes down to having clear goals at the start. Throwing stuff at the wall until it works is one way to design a game or a mod, but it generally takes a long time and can be expensive. Once I have a rough target, my inclination is always to try the extremes, and tone things back if necessary. If you try something crazy you could strike gold, or you might not, but you'll have at least learned something, and perhaps there are some ideas you can use in other ways. If absolutely nothing works, you can always fall back on what's been done before. Fortune favors the bold!

Sevo

--------

This is a harder question to answer, and I think it falls into the domain of "what makes a good mod". It is certainly easier to simply change combat values or movement or whatnot; to actually innovate an entirely new system is difficult.

Sometimes a new system works very well: for example the new Civilopedia I started with was well received because, I think, it was a marked improvement over the old system. On the other hand, I tried a new system for religions in "Faces of God" and while I spent hours and hours putting together the units, python, etc for that mod, in the end the system didn't lend itself to great game play; at least not as it stood and I haven't had time to go back and re-examine it. So there's a bit of trial and error involved, certainly, but hitting on a successful new idea is worth the losses you'll encounter.

Rhye

--------

I think that this is a concern. That makes the difference between a mod for personal use and for sharing. For instance, if I just want to boost some units stats, I will not post it. Usually I'd eventually find out another mod better than mine, that does the boost I wanted, plus something else.

I'll share it if I have a unique idea instead, something that hasn't been done before by anybody.


#### 复杂性的危险

Overly complex designs are the easiest mistake for a designer to make. A designer should differentiate between systems that are fun to design, and those that are fun to play, they are rarely the same.

Personally I am fighting this issue all of the time. I find myself designing the system the way I imagine, setting it down and coming back to it with a clear head to take a look at what I made. Most of the time I can cut a lot of the design without losing its functional purpose or flavor, or I find that the mod is better without it at all.

As in all things there is a balance here. Every new feature brings in some additional complexity, just as every new object adds to the amount of information the player needs to track as we discussed in the Danger of More section. Having the idea is only the start.

I was considering a manufacturing process for a mod. If you have access to dyes and cotton you can build a tailor shop that produces a “Cloth” resource. If you have access to fur you can build a leatherworker that produces a “Leather” resource. If you have cloth and leather in a city then all units produced in that city get leather breastplates that improve their combat ability. And on and on it went, it was a lot of fun to design, huge complex systems with interdependencies everywhere. It would have been a disaster to play for most players. That’s not to say that some people wouldn’t have enjoyed it, some people love complexity. Just keep in mind that what sounds reasonably simple in the design stages, when added together with everything else and in the hands of a player that hasn’t spent the hours considering and tinkering with the mod as you have, can be a substantial roadblock.

**Q: How do you balance between complex ideas that bring new elements to the game and the difficulty they cause casual players?**

Soren Johnson

--------

The best place to introduce complex ideas are at the fringes, in places where the player doesn't have to understand the complexities if they he or she doesn't want to. For example, many Civ4 players probably didn't realize how Great People probabilities are calculated - but not knowing the details didn't necessarily stop them from progressing and enjoying the occasional Great Person that they got naturally.

Jon Shafer

-------

I think systems can be designed in a way to be relatively approachable and yet still have depth. This was my goal with the Social Policies. Lots and lots of choices to make, but the system isn't impossible to figure out as a first-time player. Again, everyone's specific tastes vary, but I do feel it's possible to hit both ends of the spectrum if that's your goal from the beginning. You won't end up there accidentally though! I think you can have mechanics aimed at more experienced players, but they can't be core to the experience of playing the game, and new players should be able to safely ignore a feature if it's not something they're comfortable using.

Sevo

---------

I don't worry too much about adding new ideas to game play, but my mod is primarily an expansion of game play so most users follow pretty readily. Even when I change stuff I find that gamers by nature are quick to pick up new functions, if they aren't too mysterious or complicated.

Rhye

--------

I base myself on feedback. If I see that a complex change is welcome and understood anyway, it's okay. Documentation (including a site, a faq, and quick answers on the forum thread) can be important. But even more important than this, is the difficulty that it will cause me. When I add something to my schedule, I have already asked myself if it's possible to do that, and how.

Two examples of what NOT to do:

- compile a list of features and advertise it with no idea about how to do it, and
- work without a deadline, keeping postponing your work for months and months. it's true, you work for free, but if you don't finish your work you'll have really wasted your time.

### 制作模组的过程

#### 书写设计文档

It’s not fun, everyone wants to get right into making changes and seeing those changes in the game. But your first step is to get a design document written. It can be a forum post you update, a Word document or just notes on paper. Without it your mod design won’t have focus and it will be difficult to make the best use of your time without a clear idea of what needs to be done. It will also be difficult for team members to help you if they don’t have access to the full design list.

I have a hard time being creative in front of a computer so I grab a notepad and pen when I want to do serious design work. Sitting in a comfortable chair I jot down ideas and think about things I want to improve. Different people have different methods, the important part is to find one that works for you.

#### 经济性 = 缺乏资源时的选择

A game is an entertainment activity that gives us options, and rewards us for selecting them skillfully. The quality of those options, the amount of appropriate risk and reward we get from each, the variety of options (without being overwhelming) and the successful merger of function with a matching flavor determines if the game is a good one.

To make choices we need to have some cost, something we give up to gain the advantage the option offers. This could be paid in some other resource like gold or production time, or it could be in something less definable. One of the classic early game options is, do you build the infrastructure of your cities or build defenders? If you build the infrastructure you will have better cities by the mid-game, if you last that long. But the price you pay for it is in increased risk of attack and losing the game.

That is an elegant game design (thanks to Sid Meier). To have options we need to have limits, we can’t do it all so some things need to be sacrificed to gain others. That doesn’t mean that all of the options need to have a direct disadvantage, it may be enough to just have the sacrifice be that you have given up taking other paths, it is economics.

I’m particularly fond of Civ4’s civic design. Personally I like having civics that have a direct disadvantage along with whatever advantages it offers, but from a straight design perspective I have a deep appreciation for what Firaxis did. The civics don’t need disadvantages, their disadvantage is that if you pick a civic you can’t pick any of the other ones in that category.

Another truism of scarcity is that we must have good and bad elements for it to work, the Desert dilemma. From a players perspective deserts seem useless, they have a few functions but are generally the least useful of the terrains. So you may wonder why they can’t be removed and replaced with something that does more. Their design function is that they aren’t useful, and therefore by comparison other terrains are. The difference between the two makes the players strategic options more interesting.

#### 困惑时，请相信 Firaxis

The old saying is if there is a fence in the woods you should know who built it and why it was built before you tear it down. That was never more true than when making mods. I will admit, I love changing things more than I love reading about the way things work so the trap I sometimes get caught in is designing something without a good appreciation for the way its associated systems work.

My advice is before you make a significant change you review the way a similar system works in detail. If you want to make a new unitcombat, look at the ones that are already developed. Do a search in the python, xml and the SDK for matches against a similar unitcombat and see where it has been used and what it has been used for. When you understand how the data is used you will be better prepared to make your own.

The same goes for balancing. Firaxis has done more work, and has had more man hours invested in “Vanilla Civ” then we can hope to with any one mod. Don’t let all of the testing and feedback go to waste. Look at the iCombat increases between upgraded units that Civ applied when you consider your own. Base your new specialists effects on those that already exist.

#### 制定优先级，学会拒绝

The sad fact is that we all have limited time. As much as we may love to mod there are only so many hours in the day and most of us have jobs and families that also demand our time (to my wife: "I mean that we want to spend time with").

We have to be careful about our ambition. If our only goal is to mod for fun, to play around, change our game and learn a little bit in the process then this isn’t a big concern. If we are working with others and they have helped by devoting time and effort to your mod this becomes more important. In those situations there is some responsibility to produce a working mod.

So we have to realistically decide what will and won’t be done, at least for the short term. If you aren’t familiar with python or C++ then starting a mod that requires programming changes is probably going to be too much. But you could focus on xml modding only.

Once you have your idea you will need to prioritize them. I am always on the lookout for “Drool Factor”, those features or additions that will make people want to download and play the mod. No one ever downloaded a mod because it had 17 different kinds of infantry or a ice cream truck unit that played music when it drove around (well, maybe I would download that mod just to try it out) so as interesting as those ideas may be they don’t have any “Drool Factor”.

You have read countless marketing releases talking about one game or another. What features did you read about that excited you? Those are the kinds of things that should be at the top of the priority list. I hate to spend time working on a feature that players aren’t going to be excited about.

And some changes shouldn’t be made at all. This is more difficult when you are working with a team that is as excited and working as hard on the mod as you are. But the mod owner has to be the one to say what is going to go in and what isn’t. Remember it's easier to say no and then change your mind and add it later than it is to do all the work to make something only to remove it later on.

#### 组件队伍

There is no magic trick to building a team. Assume from the beginning that you will need to do all of the work yourself, then start working. If people join you along the way to help out, so much the better, but don’t expect it.

In most cases an idea, no matter how appealing, won’t draw a team by itself. Instead the mod owner will have to show the work he has done to start getting help. The reason is that there are new ideas for mods posted every day asking for help. Naturally people don’t want to spend time working on a mod idea that won’t ever be released. If they see that you have put a lot of effort into it yourself the risk that their work will go wasted is lessened and they will be more apt to join.

It is my opinion that the mod owner for a complex mod should be a programmer. He doesn’t need to be the best programmer on the team (I’m certainly not on my team) but he does need to understand the programming aspects. This is because the mod owner has to be the one to decide what is and isn’t going to go into the mod, he won’t be able to make these decisions well if he doesn’t have a decent appreciation for what has to happen “under the covers” to make it work. Also the mod owner always has to be prepared to do it alone if need be. If you leave the programming aspects to someone else without an ability to pick up if they leave you may risk losing your mod if they go on to other things.

One more point about teams. This is an open, fun, collaborative process. Don’t expect deadlines and arbitrary goals to be met unless you are willing to pay your team. It’s the mod owners responsibility to get the work that needs to be done listed and the team members can work on different aspects as they desire. Anything more than that isn’t fun for anyone. Invariably you will be working on some component and want a piece from a team member to finish it, or team members will come and go as their real lives dictate, but that is the nature of collaborative modding.

#### 何时发布

A mod is never perfect, there is always some new feature, art or object you would love to get added before you release. The danger of that is that you may never get it out.

Momentum is important for a mod, both for you (hearing from players that enjoy your mod will give you the energy to keep going) and especially for your team (who want to see their contributions out and available). Other mod makers may have different ways of doing this but I like to set hard release dates and then work back from them instead of working from feature lists. If I commit to release a new version on the first Friday of each month then you and your team know when they have to have new stuff in by to have it included.

Releasing after certain features are in creates a pressure to get work done that may aggravate your team if they feel like they are holding up the release. Better to just tell them the dates and let them manage their part as they prefer.