---
layout: post
title: Super Bomberman 5 and Controller Port Priority
color: rgb(250, 50, 50)
feature-img: "assets/img/feature-img/bomberman.jpg"
thumbnail: "assets/img/thumbnails/feature-img/bomberman.jpg"
tags: [Bug Breakdown]
---
When I was kid, there was one day that me and my three cousins were playing Super Bomberman 5 for the Super Famicom and a weird glitch happened. We used to play Tag Team mode (2v2) with a mechanic called "Revenge Cart" turned on. This mechanic basically allowed a player to have a second chance of returning to the battlefield by putting its character on top of a cart that could move around the corner of the stage. While on top of your cart, you could throw bombs in the players that were still alive and, if you manage to get a kill, you would respawn. The glitch happened because of that mechanic.

I was on top of the missile while my duo was fighting in the battlefield. Same situation for the other team, one player downed (on top of the cart) and the other one on the battlefield. So basically, one player of each team on the battlefield and one player of each team on cart. My teammate ended up dying to a bomb of the cart player of the other team (leading to this player respawing), but while my teammate was in the animation frames of dying, I managed to score a kill on the oponnent that was on the battlefield, so I respawned too.

The expected behaviour in a situation like this one is for the game to simply keep things going, one player of each team is alive and only one can be the winner, but what happened is that both me and my opponent triggered the animation victory but my team was the one that received the trophy in the end (thankfully).

I remembered this situation recently and now a days, with more knowledge and tools that help testing out those situations, I tried to recreate the glitch and take a better look at it.

## Trying to Reproduce the Bug
I recreated the situation using Mesen emulator and the feature Frame Advance, allowing me to make precise inputs the frame I want and advance frame-by-frame to see the results. In order to decide the teams, Super Bomberman 5 shows two rows for you to organize which players belong to which team. We will call the top row Team A and the bottom one Team B.

{% include aligner.html images="feature-img/bomberman5charaselect.jpg" caption="A caption under the images" %}

No matter which team got the kill first from the missile, the trophy always goes to Team B. A bomb thrown from the missile takes about ~15 frames to hit the ground and become active, once active, it always explodes after a certain time (time is always the same). Bomb explosion hitbox activates frame 4 (independent of its range) and after it hits a character, it will only trigger the invincibility frames of the oponnent's winning animation after 35 frames.

If both players from the cart throw a bomb at the exact same frame and get a kill from the only person alive from the opposite team, they are going to respawn 47 frames after the kill and the game won't glitch. One frame earlier or late and the glitch happens, caused by the window before the invencibility frames of the players are triggered.

I tried then to switch teams: Bomberman and Dave Bomber (Red Guy) are Team A, Bomber Woof and Gary Bomber (Blue Guy) are Team B. Used the same method and then..., trophy went to Team A. Now things changed, and for worse. . Is it a character specific thing? If it is I will need to test all characters and see which one is better than the other? That is going to be a lot of work but I am determined to discover why this glitch happens.

## Results and Conclusion
After several tests among the cast, I thought I got. It seemed that it was character specific, after testing several different characters combinations things started to follow a certain path and when I thought I figured it out, at the last test, the result breaks pattern that was being build and throw all the results of my tests in the trash.

Then I thought about port priority, there were other games I saw that behaved differently depending on the port the player used (Smash Bros Melee is an example), so I thought it was worth a try. I kept testing for a couple of hours and that seems to be the issue, the game prioritizes ports in the following order: Port 4 > Port 3 > Port 2 > Port 1. So, if player a team is composed by player 3 and player 4, they will always have advantage when one of them respawns from the cart (even if their duo died some frames prior to the respawn). The priority applies for the player that gets the kill.

This situation is unlikely but it led to a really good moment when it happened while I was playing. I will certainly take a look at the other games of this franchise and seek for other glitches on them.
