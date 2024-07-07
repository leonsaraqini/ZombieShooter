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

MainTimerEvent <br />
Handles the main game loop, including player movement, zombie movement, collision detection, health updates, score updates, and game over condition.<br />

```
private void MainTimerEvent(object sender, EventArgs e)
{
    if (playerHealth > 1)
    {
        healthBar.Value = playerHealth;
    }
    else
    {
        gameOver = true;
        player.Image = Properties.Resources.dead;
        GameTimer.Stop();

        DialogResult result = MessageBox.Show($"Game over!\nYour Score: {score}\nDo you want to play a new game?", "Game Over", MessageBoxButtons.YesNo, MessageBoxIcon.Question);

        if (result == DialogResult.No)
        {
            Application.Exit();
        }
        else
        {
            RestartGame();
        }
    }

    txtAmmo.Text = "Ammo: " + ammo;
    txtScore.Text = "Kills: " + score;

    if (goLeft == true && player.Left > 0)
    {
        player.Left -= speed;
    }
    if (goRight == true && player.Left + player.Width < this.ClientSize.Width)
    {
        player.Left += speed;
    }
    if (goUp == true && player.Top > 45)
    {
        player.Top -= speed;
    }
    if (goDown == true && player.Top + player.Height < this.ClientSize.Height)
    {
        player.Top += speed;
    }

    foreach (Control x in this.Controls)
    {
        if (x is PictureBox && (string)x.Tag == "ammo")
        {
            if (player.Bounds.IntersectsWith(x.Bounds))
            {
                this.Controls.Remove(x);
                ((PictureBox)x).Dispose();
                ammo += 5;
            }
        }

        if (x is PictureBox && (string)x.Tag == "zombie")
        {
            if (player.Bounds.IntersectsWith(x.Bounds))
            {
                playerHealth -= 1;
            }

            if (x.Left > player.Left)
            {
                x.Left -= zombieSpeed;
                ((PictureBox)x).Image = Properties.Resources.zleft;
            }
            if (x.Left < player.Left)
            {
                x.Left += zombieSpeed;
                ((PictureBox)x).Image = Properties.Resources.zright;
            }
            if (x.Top > player.Top)
            {
                x.Top -= zombieSpeed;
                ((PictureBox)x).Image = Properties.Resources.zup;
            }
            if (x.Top < player.Top)
            {
                x.Top += zombieSpeed;
                ((PictureBox)x).Image = Properties.Resources.zdown;
            }
        }

        foreach (Control j in this.Controls)
        {
            if (j is PictureBox && (string)j.Tag == "bullet" && x is PictureBox && (string)x.Tag == "zombie")
            {
                if (x.Bounds.IntersectsWith(j.Bounds))
                {
                    score++;

                    this.Controls.Remove(j);
                    ((PictureBox)j).Dispose();
                    this.Controls.Remove(x);
                    ((PictureBox)x).Dispose();
                    zombiesList.Remove(((PictureBox)x));
                    MakeZombies();
                }
            }
        }
    }
}
```

KeyIsDown<br />
Handles player movement input by setting direction flags and changing the player's image based on the direction.<br />
```
private void KeyIsDown(object sender, KeyEventArgs e)
{
    if (gameOver == true)
    {
        return;
    }

    if (e.KeyCode == Keys.Left)
    {
        goLeft = true;
        facing = "left";
        player.Image = Properties.Resources.left;
    }

    if (e.KeyCode == Keys.Right)
    {
        goRight = true;
        facing = "right";
        player.Image = Properties.Resources.right;
    }

    if (e.KeyCode == Keys.Up)
    {
        goUp = true;
        facing = "up";
        player.Image = Properties.Resources.up;
    }

    if (e.KeyCode == Keys.Down)
    {
        goDown = true;
        facing = "down";
        player.Image = Properties.Resources.down;
    }
}
```
KeyIsUp<br />
Handles player movement input and shooting bullets. Also listens for the Enter key to restart the game after a game over.<br />
```
private void KeyIsUp(object sender, KeyEventArgs e)
{
    if (e.KeyCode == Keys.Left)
    {
        goLeft = false;
    }

    if (e.KeyCode == Keys.Right)
    {
        goRight = false;
    }

    if (e.KeyCode == Keys.Up)
    {
        goUp = false;
    }

    if (e.KeyCode == Keys.Down)
    {
        goDown = false;
    }

    if (e.KeyCode == Keys.Space && ammo > 0 && gameOver == false)
    {
        ammo--;
        ShootBullet(facing);

        if (ammo < 1)
        {
            DropAmmo();
        }
    }

    if (e.KeyCode == Keys.Enter && gameOver == true)
    {
        RestartGame();
    }
}
```

ShootBullet<br />
Creates and shoots a bullet in the specified direction.<br />
```
private void ShootBullet(string direction)
{
    Bullet shootBullet = new Bullet();
    shootBullet.direction = direction;
    shootBullet.bulletLeft = player.Left + (player.Width / 2);
    shootBullet.bulletTop = player.Top + (player.Height / 2);
    shootBullet.MakeBullet(this);
}
```

MakeZombies<br />
Creates a new zombie at a random location and adds it to the game.<br />

```
private void MakeZombies()
{
    PictureBox zombie = new PictureBox();
    zombie.Tag = "zombie";
    zombie.Image = Properties.Resources.zdown;
    zombie.Left = randNum.Next(0, 900);
    zombie.Top = randNum.Next(0, 800);
    zombie.SizeMode = PictureBoxSizeMode.AutoSize;
    zombiesList.Add(zombie);
    this.Controls.Add(zombie);
    player.BringToFront();
}
```
DropAmmo<br />
Drops ammo at a random location in the game.<br />

```
private void DropAmmo()
{
    PictureBox ammo = new PictureBox();
    ammo.Image = Properties.Resources.ammo_Image;
    ammo.SizeMode = PictureBoxSizeMode.AutoSize;
    ammo.Left = randNum.Next(10, this.ClientSize.Width - ammo.Width);
    ammo.Top = randNum.Next(60, this.ClientSize.Height - ammo.Height);
    ammo.Tag = "ammo";
    this.Controls.Add(ammo);

    ammo.BringToFront();
    player.BringToFront();
}
```
RestartGame<br />
Restarts the game by resetting player health, score, ammo, and spawning new zombies.<br />
```
private void RestartGame()
{
    player.Image = Properties.Resources.up;

    foreach (PictureBox i in zombiesList)
    {
        this.Controls.Remove(i);
    }

    zombiesList.Clear();

    for (int i = 0; i < 3; i++)
    {
        MakeZombies();
    }

    goUp = false;
    goDown = false;
    goLeft = false;
    goRight = false;
    gameOver = false;

    playerHealth = 100;
    score = 0;
    ammo = 10;

    GameTimer.Start();
}
```
# Team Members
- Leon Saraqini (211508)
- Ariton Spahiu (211552)



