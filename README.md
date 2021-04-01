# Montezuma's Revenge

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
While both the lives as well as the points and the red and purple key state indicators are self-explanatory, the x,y, and vx and vy state indicators are less intuitive. To update the player’s position, or the x and y state indicators, I use the function `updatePlayer()` to detect any user inputs, and update the player’s position accordingly:

```lua
function updatePlayer()
	--Player Input
	if btn(2) and not Plr.damaged then 
		Plr.vx=-1
	elseif btn(3) and not Plr.damaged then 
		Plr.vx=1
 	else 
		Plr.vx=0 
	end
	
	if btn(0) and Plr.vy==0 and Plr.grounded then 
		Plr.vy=-2.7
	end
	
	-- Code Handling Collisions
	
	--Update Player Position
	Plr.x=Plr.x+Plr.vx
 	Plr.y=Plr.y+Plr.vy
end
```
When the player presses a specific button, instead of immediately moving the player there, a request to move in that direction is made by the function setting a specific value to `Plr.vx` or `Plr.vy`, which is the corresponding displacement of the player requested; if either the left or right button is pressed, the displacement is set to -1 or 1 respectively, which corresponds to the movement of the character to the right or left, while if the up button is pressed, the player's displacement on the y axis is set to -2.7. The request is then vetted by a string of collision-handling if statements which use the function `solid(x,y)` to check whether the tile at said coordinates is solid or not. 

#### Entities and the Map
To make the level creation simple, I wanted to make a system which would handle all the entity behaviors for me behind the scenes while I focus purely on level design. To do that, I wrote the function `initialEntityScan()`, which scans every tile on the current map, saves each tile that has an ID above a certain number in an array called `ENTITY`, and then overrides the tile taken with `mset()`. An entity in the `ENTITY` array is saved in the following format: 
```lua
ENTITY[i][spriteNum,x,y,alive,levelNo,xdirection]
```
...where the `spriteNum` is the sprrite number of the entity, `x` and `y` are the current position, `alive` is the boolean flag that determines whether the entity exists and `levelNo` is the level at which the entity is rendered in. 

From there, the function `updateEntities()` goes through every item in `ENTITY` and handles the specified behavior of each entity based on its tile ID:
```lua
function updateEntities()
	for i in pairs(ENTITY)do
		if ENTITY[i][5]==Game.levelNo then
			--Calculate (x,y) positions of entities
			entityX=(ENTITY[i][2]-levelToX(Game.levelNo))*8
			entityY=(ENTITY[i][3]-levelToY(Game.levelNo))*8
			--Check whether player intercepts the entity
			xField=Plr.x>=entityX-4 and Plr.x<=entityX+4 
			yField=Plr.y>=entityY-4 and Plr.y<=entityY+4
			if ENTITY[i][1]==149 then yField=Plr.y>=entityY-4 and Plr.y<=entityY+12 end
		
			--Handling Enemies
			if ENTITY[i][1]<=138 then
				if ENTITY[i][4]>0 then
					--Handling Player Interaction
					if xField and yField and ENTITY[i][1]<=134 then
						damagePlayer(1)
					end
					--Handling Position and Animation
					updateEnemy(i)
				end
			--Handle Item Pickup and Display of Pickup
			else 
				if ENTITY[i][4]>0 then
					if xField and yField then
						if playerPickup(ENTITY[i][1]) then
							ENTITY[i][4]=0
						end
					end
					
					--Display
					spr(ENTITY[i][1],entityX,entityY,0)
				end
			end
		end
	end
end
```
Enemy movement is handled in the `updateEnemy(i)` function, which does the exact same thing that `updatePlayer()` did, except instead of player input, a loop of pre-made movement is played over and over, and the pickups react in preset ways to player interaction. 

By handing things this way, I was able to design my levels purely through the Map interface, which then later was scanned and updated according to preset rules assigned to the specific tile IDs. This simplified the process, and made the level design process completely not connected to writing any code; I was able to not touch any of my code for the majority of the level design process.

#### What is an entity?
To understand what is and what isn't considered an entity, it's best to understand all the different types of tiles present in the game. While the tiles 0 through 30 have an effect on the player, they are not considered to be entities, rather they are the **solid** tiles. Tiles with the ID 32, which are ladders, tiles between 48 and 111 are not treated as entities either, rather they are considered to be non-solid tiles which don't affect the player in any way. Lastly, tiles between 112 and 149 are considered to be entities, with tiles from 112 to 123 being moving enemies, tiles from 128 to 135 being stationary enemies, or tiles from 144 to 149 being pickable entities, which upon the interaction with a player dissapear and change the state of the player (add points, add health etc...). The tile that is most confusing, and incorporates almost all the aspects of the tiles is the door tile.

Doors in my implementation are considered entities, but they are also solid objects that interact both with the player and other tiles on the screen. The closed door tile (either 136 or 138) does three things: it acts as a solid tile if any entity or player tries to pass through, it switches from closed to open if a player touches it, has a correct key, and presses z, and it switches from closed to open if a tile above or below it is the open door tile (137). That way, when the door is closed, the player nor any entities cannot pass, but when the player opens any of the tiles in the number of tiles that the door is made out of, the entire door opens permanently. This is done with `mset()`, and the door tile is permanently opened.

#### Level Design
In remaking this game, I wanted to emulate the feeling of being lost that the original game instilled in players; it was an unforgiving game that forced you to replay it multiple times to make progress. To do that, I decided to make the game have 3 distinctive areas: the **stone area**, which is both what the starting area, which focuses on introducing the main concepts of the game to the player, and the maze area looks like, which focuses on the exploration through mazes as well as some light platforming using ladders, the **„red” or fire area**, which focuses on lasers which turn off and on which the player needs to avoid, and the **„green” or plant area**, which focuses on platforming, both on regular platforms and platforms that move the player to one direction, as well as finding the path that a player can jump through or fall on. While the original game focused on exploration down, with the player constantly descending deeper and deeper into the depths of the temple, I wanted to diversify my version; I made the maze area feel claustrophobic and descending downwards, the green area climbing higher and higher, and then asking the player to fall down large distances, 
