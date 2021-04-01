# Montezuma-s-Revenge

### Atari 2600

**Atari (or more accurately Atari 2600)** was a gaming console released in September of 1977. Originally called the **Atari Video Computer System**, it popularized the use of ROM cartridges for storing game data. It came with a Joystick and a pair of paddle controllers, which were essentially round turning wheels that would control the player on a specified axis along with one or more „fire” buttons. The game that made the Atari 2600 extremely successful was one of the low-resolution games that came with the system, namely **Space Invaders (1980)**, and then later **Pac-Man (1982)** but by 1983, the studio failed to follow up with any game as successful as said two games. The year is also well-known for the large-scale recession of the video games market that lasted from 1983 up until 1985, which one of the major factors of was the failure to follow up the two titles mentioned before. 

### Montezuma's Revenge

In 1983, a sixteen year old programmer **Robert Jaeger** (who by now has written titles such as Chomper and Pinhead, clones of Pac-Man and Kick Man respectively) decides to show off his new creation in a consumer electronics convention, **Montezuma’s Revenge**. The game peaked interest of Parker Parker Brothers, and the game was released in 1984. Montezuma’s Revenge, which references travelers diarrhea, is a very early example of a game now classified as **Metroidvania**, with many people pointing to it being the first example of such game. 

**Metroidvania**, which comes from the combination of the titles of two most influential and emblematic games of said format, namely Metroid and Castlevania: Symphony of the Night, is genre of games that is usually a 2D platformer with one continuous map, a large emphasis on exploration, puzzle solving, as well as a system of locks and keys which through the player slowly gains access to a larger percentage of the map. The system of locks and keys can be, although does not have to be literal; player’s progress can be blocked by a literal door with a key needed to open it, but it can also be a ledge that’s too high to climb that requires the player to gain a specific ability, or any other limitation of the sort. Although a Metroidvania does not have to have all said qualities to be a Metroidvania, Montezuma’s revenge has all of them present; the game’s main concept is the exploration of a 16th century Aztec temple of emperor Montezuma II filled with puzzles, obstacles such as closed doors or enemies, and a continuous map that is revealed to the player as they explore it. The player has a set amount of lives, and when the player dies, they have to start from scratch. There are many pickups available for the player such as health pickups, or points, which slowly open up the temple to the player.

### Remaking of Montezuma's Revenge
#### Player
One of the first steps in remaking Montezuma’s revenge was to make my character. While in the original, the character is called Panama Joe, or Pedro, and wore a Sombrero hat, I decided to go with a less racially-charged character design, settling on a character that resembled a cartoon „thief” or pirate: wearing a dark purple bandana with a grey shirt and blue pants. After animating the character, I then handled the player movement. To do that, I create a Player "object", which in reality is a list of variables, where I store all the necessary information about the player's state:

```lua
Plr={
	lives=5,
	x=20,y=70,
	vx=0,vy=0,
	
	-- Other state indicators connected to animation
	
	Rkeys=0,
	Pkeys=0,
	points=0
}
```


#### Level Design
In remaking this game, I wanted to emulate the feeling of being lost that the original game instilled in players; it was an unforgiving game that forced you to replay it multiple times to make progress. To do that, I decided to make the game have 3 distinctive areas: the stone area, which is both what the starting area looks like, which focuses on introducing the main concepts of the game to the player, and the maze area, which focuses on the exploration through mazes as well as some light platforming using ladders, the „red” or fire area, which focuses on lasers which turn off and on which the player needs to avoid, and the „green”, or plant area, which focuses on platforming, both on regular platforms and platforms that move the player to one direction, as well as finding the path that a player can jump through or fall on. 
