# dropper-game
dropper game. bij elke pizza die je vangt gaat je score met 1 omhoog je wint als je 100 punten hebt. zodra je 20 punten hebt wordt de valsnelheid verdubbeld. als je meer dan 5 pizza's laat vallen is het game over.



source code


using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace MAMA_MIA_THAT_S_A_SPICY_MEATBALL
{
    public partial class Form1 : Form
    {
        bool naarlinks;
        bool naarrechts;
        int snelheid = 8;
        int score = 0;
        int gemist = 0;
        Random rndY = new Random(); // willekeurig Y coordinaat 
        Random rndX = new Random(); // willekeurig X coordinaat
        PictureBox splash = new PictureBox();


        public Form1()
        {
            InitializeComponent();

            reset();
        }
        
        
            
        

        private void keyisdown(object sender, KeyEventArgs e)
        {
            if (e.KeyCode == Keys.Left)
            {
                naarlinks = true;
            }
            if (e.KeyCode == Keys.Right)
            {
                naarrechts = true;
            }
        }

        private void keyisup(object sender, KeyEventArgs e)
        {
            if (e.KeyCode == Keys.Left)
            {
                naarlinks = false;
            }
            if (e.KeyCode == Keys.Right)
            {
                naarrechts = false;
            }
        }

        private void gameTick(object sender, EventArgs e)
        {
            label1.Text = "pizza's gebaken : " + score;
            label2.Text = "keren gefaald : " + gemist;

            if (naarlinks == true && boyardee.Left > 0)
            {
                boyardee.Left -= 12;
                boyardee.Image = Properties.Resources.rsz_boiardi2;
            }
            if(naarrechts == true && boyardee.Left + boyardee.Width < this.ClientSize.Width)
            {
                boyardee.Left += 12;
                boyardee.Image = Properties.Resources.rsz_boiardi2;
            }
            
            foreach (Control X in this.Controls)
            {
                if (X is PictureBox && X.Tag == "pizza")
                {
                    X.Top += snelheid;
                

                if(X.Top + X.Height > this.ClientSize.Height)
                {
                    splash.Image = Properties.Resources.rsz_idc;
                    splash.Location = X.Location;
                    splash.Height = 59;
                    splash.Width = 60;
                    splash.BackColor = System.Drawing.Color.Transparent;

                    this.Controls.Add(splash);

                    X.Top = rndY.Next(80, 300) * -1;
                    X.Left = rndX.Next(5, this.ClientSize.Width - X.Width);
                    gemist++;
                }

                if (X.Bounds.IntersectsWith(boyardee.Bounds))
                {
                    X.Top = rndY.Next(100, 300) * -1;
                    X.Left = rndX.Next(5, this.ClientSize.Width - X.Width);
                    score++;
                }

                if(score >= 20)
                {
                    snelheid = 16;
                }
                if(gemist > 5)
                {
                    MessageBox.Show("das kut he, je pizza droom is voorbij" + "\r\n" + "klik op OK om opnieuw te starten");
                    reset();
                }
                
                if(score >= 100)
                    {
                        MessageBox.Show("eyyy ik ben trots op je " + "\r\n" + "klik op OK voor nog een rondje ..... jij bent de pizzakoning!");
                        reset();
                    }
                    


                }
            }
        }

        private void reset()
        {
            foreach (Control X in this.Controls)
            {
                if (X is PictureBox && X.Tag == "pizza")
                {
                    X.Top = rndY.Next(80, 300) * -1;
                    X.Left = rndX.Next(5, this.ClientSize.Width - X.Width);
                }
            }
            boyardee.Left = this.ClientSize.Width / 2;
            boyardee.Image = Properties.Resources.rsz_boiardi2;
            score = 0;
            gemist = 0;
            snelheid = 8;

            naarlinks = false;
            naarrechts = false;
            GameTimer.Start();
        }

    }
}
