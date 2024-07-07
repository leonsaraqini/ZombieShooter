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

