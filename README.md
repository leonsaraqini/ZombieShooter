# Zombie Shooter Game

The goal of this application is to implement a simple 2D zombie shooter game. The game features movement and shooting functionalities. The controls are straightforward, with restricted movements. The gameplay is simple: aim to destroy as many zombies as possible and survive their attacks.

Upon starting the game, the player is introduced to the main gameplay screen with a top-down view of the player character and zombies. The player can move the character and shoot at the zombies. The interface shows the player's ammo count, kill score, and health bar.

![Screenshot_1](https://github.com/leonsaraqini/ZombieShooter/assets/73612089/d7b5ccb5-2fc7-4688-af50-ac016c31efe8)

![Screenshot_2](https://github.com/leonsaraqini/ZombieShooter/assets/73612089/7af49f4a-013e-4d2c-ab95-9bbced362fe9)

# Controls

Left Arrow: Move left <br />
Right Arrow: Move right <br />
Up Arrow: Move up <br />
Down Arrow: Move down <br />
Spacebar: Shoot <br />
Ok Button: Restart game after game over <br />

# Rules
You have 100 health points. Health decreases by 1 each time a zombie touches the player. <br />
Zombies move towards the player and attack. <br />
Ammo is limited but can be replenished by picking up ammo boxes scattered around the map. <br />
The game ends when the player's health reaches zero. <br />
The objective is to kill as many zombies as possible. Each kill adds to your score. <br />

The game does not use constants explicitly, but key properties include: <br />

Form size and dimensions. <br /> 
Player's speed and health. <br />
Zombie's speed. <br />
Ammo count. <br />
Timer interval for game updates. <br />

# Fields

goLeft, goRight, goUp, goDown: Flags for player movement. <br />
gameOver: Flag to indicate if the game is over. <br />
facing: Direction the player is facing. <br />
playerHealth: Health of the player. <br />
speed: Movement speed of the player. <br />
ammo: Ammunition count. <br />
zombieSpeed: Movement speed of the zombies. <br />
randNum: Instance of Random for generating random numbers. <br />
score: Player's score (number of zombies killed). <br />
zombiesList: List of zombies in the game. <br />

# Methods

