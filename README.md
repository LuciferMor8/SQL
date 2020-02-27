using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Data.OleDb;
using System.Drawing;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace WindowsFormsApplication1
{

    public partial class Form1 : Form
    {
        
        public static string connectString = "Provider=Microsoft.ACE.OLEDB.12.0;Data Source=tea1.accdb;";
        private OleDbConnection myConnection; 
        public Form1()
        {
            InitializeComponent();
            button4.Visible = false;
            myConnection = new OleDbConnection(connectString);
            // Відкриває ДБ
            myConnection.Open();
        }
        int Button_check_add = 0;

        void name_sub(string[] num1)
        {
            for (int m = 0; m < num1.Length; m++)
            {
                string query = "SELECT * FROM sub where id=" + num1[m].ToString() + "";
                OleDbCommand command = new OleDbCommand(query, myConnection);
                // получаємо обєкт OleDbDataReader для читання табличного результату запиту SELECT
                OleDbDataReader reader = command.ExecuteReader();
                for (int i = 0; reader.Read(); i++)
                {
                    label5.Text += reader[1].ToString()+"\n";
                }
            }
        }

        private void button1_Click(object sender, EventArgs e)
        {
            button4.Visible = true;
            listBox2.Items.Clear();
            string query = "SELECT * FROM VUKL order by id ";
            label1.Visible = true;
            label4.Visible = false;
            OleDbCommand command = new OleDbCommand(query, myConnection);
            // получаємо обєкт OleDbDataReader для читання табличного результату запиту SELECT
            OleDbDataReader reader = command.ExecuteReader();
            for (int i = 0; reader.Read(); i++)
            {
                listBox2.Items.Add(reader[1].ToString());
            }
            Button_check_add = 1;
            reader.Close();
        }

        private void button2_Click(object sender, EventArgs e)
        {
            button4.Visible = true;
            listBox2.Items.Clear();
            string query = "SELECT * FROM sub order by id ";
            OleDbCommand command = new OleDbCommand(query, myConnection);
            // получаємо обєкт OleDbDataReader для читання табличного результату запиту SELECT
            OleDbDataReader reader = command.ExecuteReader();
            for (int i = 0; reader.Read(); i++)
            {
                listBox2.Items.Add(reader[1].ToString());
            }
            label1.Visible = false;
            label4.Visible = true;
            Button_check_add = 2;

        }

        private void button3_Click(object sender, EventArgs e)
        {
            button4.Visible = true;
            listBox2.Items.Clear();
            string query = "SELECT * FROM student order by id ";
            label1.Visible = true;
            label4.Visible = false;
            OleDbCommand command = new OleDbCommand(query, myConnection);
            // получаємо обєкт OleDbDataReader для читання табличного результату запиту SELECT
            OleDbDataReader reader = command.ExecuteReader();
            for (int i = 0; reader.Read(); i++)
            {
                listBox2.Items.Add(reader[1].ToString());
            }
            Button_check_add = 3;
        }

        private void button4_Click(object sender, EventArgs e)
        {
            if (Button_check_add == 1) { 
            Form2 add = new Form2();
            add.Show();
            }
            if (Button_check_add == 2) {
                Form3 add = new Form3();
                add.Show(); 
            }
            if (Button_check_add == 3)
            {
                Form4 add = new Form4();
                add.Show();
            }
        }

        private void listBox2_SelectedIndexChanged(object sender, EventArgs e)
        {
            try
            {
            

            if (Button_check_add == 1)
            {
                label5.Text ="";
                string sub;
                string[] words;
                int selectedIndex = listBox2.SelectedIndex;
                Object selectedItem = listBox2.SelectedItem;
                // Цей запит, щоб виводити Жанри
                string query = "SELECT * FROM VUKL where name='" + selectedItem.ToString() + "'";
                OleDbCommand command = new OleDbCommand(query, myConnection);
                OleDbDataReader dr = command.ExecuteReader();
                //якщо значення dr існує, тобто якщо існує елемент selectedItem, то виведеться інформацію про книгу
                
                if (dr.HasRows)
                {
                    dr.Read();
                    label2.Text = dr[1].ToString();
                    sub = dr[2].ToString();
                    words = sub.Split(';');
                    name_sub(words);
                }
           }
            if (Button_check_add == 2)
            {
                label5.Text = "";
                int selectedIndex = listBox2.SelectedIndex;
                Object selectedItem = listBox2.SelectedItem;
                // Цей запит, щоб виводити Жанри
                string query = "SELECT * FROM sub where name='" + selectedItem.ToString() + "'";
                OleDbCommand command = new OleDbCommand(query, myConnection);
                OleDbDataReader dr = command.ExecuteReader();
                //якщо значення dr існує, тобто якщо існує елемент selectedItem, то виведеться інформацію про книгу

                if (dr.HasRows)
                {
                    dr.Read();
                    label2.Text = dr[1].ToString();
                }
            }
            if (Button_check_add == 3)
            {
                label5.Text = "";
                string sub;
                string[] words;
                int selectedIndex = listBox2.SelectedIndex;
                Object selectedItem = listBox2.SelectedItem;
                // Цей запит, щоб виводити Жанри
                string query = "SELECT * FROM student where lastname='" + selectedItem.ToString() + "'";
                OleDbCommand command = new OleDbCommand(query, myConnection);
                OleDbDataReader dr = command.ExecuteReader();
                //якщо значення dr існує, тобто якщо існує елемент selectedItem, то виведеться інформацію про книгу

                if (dr.HasRows)
                {
                    dr.Read();
                    label2.Text = dr[1].ToString();
                    sub = dr[4].ToString();
                    words = sub.Split(';');
                    name_sub(words);
                }
            }
            }
            catch
            {
                MessageBox.Show("Оберіть викладача", "Помилка", MessageBoxButtons.OK, MessageBoxIcon.Warning);
            }
        }
    }
}



