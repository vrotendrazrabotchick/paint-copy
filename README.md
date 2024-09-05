this code :p
using System;
using System.Drawing;
using System.Windows.Forms;

namespace WindowsFormsApp4
{
    public partial class Form1 : Form
    {
        private Color currentColor = Color.Black;
        private int brushSize = 5;
        private Bitmap bitmap;
        private Graphics graphics;
        private Point previousPoint;

        public Form1()
        {
            InitializeComponent();
        }

        private void Form1_Load(object sender, EventArgs e)
        {
            pictureBox1.Width = this.ClientSize.Width;
            pictureBox1.Height = this.ClientSize.Height;
            bitmap = new Bitmap(pictureBox1.Width, pictureBox1.Height);
            graphics = Graphics.FromImage(bitmap);
            graphics.Clear(Color.White);
            pictureBox1.Image = bitmap;
        }

        private void pictureBox1_MouseDown(object sender, MouseEventArgs e)
        {
            if (e.Button == MouseButtons.Left)
            {
                previousPoint = e.Location;
            }
        }

        private void pictureBox1_MouseMove(object sender, MouseEventArgs e)
        {
            if (e.Button == MouseButtons.Left)
            {
                Draw(e.Location);
                previousPoint = e.Location;
            }
        }

        private void Draw(Point location)
        {
            using (Pen pen = new Pen(currentColor, brushSize))
            using (Graphics g = Graphics.FromImage(bitmap))
            {
                g.DrawLine(pen, previousPoint, location);
            }
            pictureBox1.Invalidate();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            ColorDialog colorDialog = new ColorDialog();
            if (colorDialog.ShowDialog() == DialogResult.OK)
            {
                currentColor = colorDialog.Color;
            }
        }

        private void button2_Click(object sender, EventArgs e)
        {
            SaveFileDialog saveFileDialog = new SaveFileDialog();
            saveFileDialog.Filter = "PNG files (*.png)|*.png|All files (*.*)|*.*";
            if (saveFileDialog.ShowDialog() == DialogResult.OK)
            {
                bitmap.Save(saveFileDialog.FileName);
            }
        }

        private void textBox1_TextChanged(object sender, EventArgs e)
        {
            if (int.TryParse(textBox1.Text, out int size))
            {
                brushSize = size;
            }
        }
    }
} 
