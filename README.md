using System;
using System.Drawing;
using System.Windows.Forms;

namespace TicTacToe
{
    public partial class Form1 : Form
    {
        private Button[,] buttons = new Button[3, 3];
        private bool isXTurn = true;
        private int movesCount = 0;

        public Form1()
        {
            InitializeComponent();
            CreateGameBoard();
        }

        private void CreateGameBoard()
        {
            this.Text = "Крестики-нолики";
            this.Size = new Size(420, 500);
            this.StartPosition = FormStartPosition.CenterScreen;
            this.BackColor = Color.White;

            int buttonSize = 100;
            int margin = 10;
            int startX = 40;
            int startY = 40;

            for (int row = 0; row < 3; row++)
            {
                for (int col = 0; col < 3; col++)
                {
                    Button btn = new Button();
                    btn.Font = new Font("Arial", 24, FontStyle.Bold);
                    btn.Size = new Size(buttonSize, buttonSize);
                    btn.Location = new Point(
                        startX + col * (buttonSize + margin),
                        startY + row * (buttonSize + margin)
                    );
                    btn.Tag = $"{row},{col}";
                    btn.Click += CellButton_Click;

                    buttons[row, col] = btn;
                    this.Controls.Add(btn);
                }
            }

            Button restartButton = new Button();
            restartButton.Text = "Начать заново";
            restartButton.Font = new Font("Arial", 12, FontStyle.Bold);
            restartButton.Size = new Size(320, 50);
            restartButton.Location = new Point(40, 370);
            restartButton.BackColor = Color.LightGray;
            restartButton.Click += RestartButton_Click;
            this.Controls.Add(restartButton);
        }

        private void CellButton_Click(object sender, EventArgs e)
        {
            Button clickedButton = sender as Button;

            if (clickedButton.Text != "")
                return;

            clickedButton.Text = isXTurn ? "X" : "O";
            movesCount++;

            if (CheckWinner())
            {
                MessageBox.Show($"Победил игрок {(isXTurn ? "X" : "O")}!");
                DisableBoard();
                return;
            }

            if (movesCount == 9)
            {
                MessageBox.Show("Ничья!");
                return;
            }

            isXTurn = !isXTurn;
        }

        private bool CheckWinner()
        {
            for (int i = 0; i < 3; i++)
            {
                if (buttons[i, 0].Text != "" &&
                    buttons[i, 0].Text == buttons[i, 1].Text &&
                    buttons[i, 1].Text == buttons[i, 2].Text)
                    return true;

                if (buttons[0, i].Text != "" &&
                    buttons[0, i].Text == buttons[1, i].Text &&
                    buttons[1, i].Text == buttons[2, i].Text)
                    return true;
            }

            if (buttons[0, 0].Text != "" &&
                buttons[0, 0].Text == buttons[1, 1].Text &&
                buttons[1, 1].Text == buttons[2, 2].Text)
                return true;

            if (buttons[0, 2].Text != "" &&
                buttons[0, 2].Text == buttons[1, 1].Text &&
                buttons[1, 1].Text == buttons[2, 0].Text)
                return true;

            return false;
        }

        private void DisableBoard()
        {
            for (int row = 0; row < 3; row++)
            {
                for (int col = 0; col < 3; col++)
                {
                    buttons[row, col].Enabled = false;
                }
            }
        }

        private void RestartButton_Click(object sender, EventArgs e)
        {
            isXTurn = true;
            movesCount = 0;

            for (int row = 0; row < 3; row++)
            {
                for (int col = 0; col < 3; col++)
                {
                    buttons[row, col].Text = "";
                    buttons[row, col].Enabled = true;
                }
            }
        }
    }
}
