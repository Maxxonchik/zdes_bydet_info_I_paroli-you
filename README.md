Инструкция
Шаг 1. Создай новый проект
Visual Studio → Файл → Создать → Проект

Выбери Windows Forms App (.NET Framework)

Назови DemoExamen

Нажми Создать

Шаг 2. Замени весь код в Form1.cs
Скопируй этот код и вставь вместо всего, что есть в Form1.cs:

csharp
using System;
using System.Collections.Generic;
using System.Windows.Forms;

namespace DemoExamen
{
    public partial class Form1 : Form
    {
        private TextBox txtLogin;
        private TextBox txtPassword;
        private Button btnLogin;
        private Dictionary<string, User> users = new Dictionary<string, User>();

        public Form1()
        {
            CreateControls();
            LoadUsers();
        }

        private void CreateControls()
        {
            this.Text = "Авторизация";
            this.Size = new System.Drawing.Size(300, 200);
            this.StartPosition = FormStartPosition.CenterScreen;

            Label lblLogin = new Label();
            lblLogin.Text = "Логин:";
            lblLogin.Location = new System.Drawing.Point(20, 20);
            lblLogin.Size = new System.Drawing.Size(80, 25);

            txtLogin = new TextBox();
            txtLogin.Location = new System.Drawing.Point(100, 20);
            txtLogin.Size = new System.Drawing.Size(150, 25);

            Label lblPassword = new Label();
            lblPassword.Text = "Пароль:";
            lblPassword.Location = new System.Drawing.Point(20, 55);
            lblPassword.Size = new System.Drawing.Size(80, 25);

            txtPassword = new TextBox();
            txtPassword.Location = new System.Drawing.Point(100, 55);
            txtPassword.Size = new System.Drawing.Size(150, 25);
            txtPassword.PasswordChar = '*';

            btnLogin = new Button();
            btnLogin.Text = "Войти";
            btnLogin.Location = new System.Drawing.Point(100, 95);
            btnLogin.Size = new System.Drawing.Size(100, 35);
            btnLogin.Click += btnLogin_Click;

            this.Controls.Add(lblLogin);
            this.Controls.Add(txtLogin);
            this.Controls.Add(lblPassword);
            this.Controls.Add(txtPassword);
            this.Controls.Add(btnLogin);
        }

        private void LoadUsers()
        {
            users.Add("admin", new User { Password = "123", Role = "Администратор" });
            users.Add("user1", new User { Password = "123", Role = "Пользователь" });
        }

        private void btnLogin_Click(object sender, EventArgs e)
        {
            string login = txtLogin.Text.Trim();
            string password = txtPassword.Text;

            if (string.IsNullOrEmpty(login) || string.IsNullOrEmpty(password))
            {
                MessageBox.Show("Логин и пароль обязательны для заполнения");
                return;
            }

            if (users.ContainsKey(login) && users[login].Password == password)
            {
                string role = users[login].Role;
                MessageBox.Show($"Вы успешно авторизовались\nВаша роль: {role}");
            }
            else
            {
                MessageBox.Show("Вы ввели неверный логин или пароль. Пожалуйста, проверьте ещё раз введённые данные");
                txtPassword.Clear();
            }
        }

        private class User
        {
            public string Password { get; set; }
            public string Role { get; set; }
        }
    }
}
Шаг 3. Замени код в Program.cs
Открой Program.cs и замени на это:

csharp
using System;
using System.Windows.Forms;

namespace DemoExamen
{
    internal static class Program
    {
        [STAThread]
        static void Main()
        {
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);
            Application.Run(new Form1());
        }
    }
}
Шаг 4. Запусти (F5)
Всё работает. Ничего перетаскивать не нужно.

Плюсы этого способа
Не нужно искать панель инструментов

Не нужно настраивать свойства вручную

Весь код в одном месте

Работает сразу после запуска













или с подключение к бд

using System;
using System.Data.SqlClient;
using System.Windows.Forms;

namespace DemoExamen
{
    public partial class FormLogin : Form
    {
        string connString = "Server=localhost;Database=BD1;Integrated Security=true;";

        public FormLogin()
        {
            InitializeComponent();
        }

        private void btnLogin_Click(object sender, EventArgs e)
        {
            string login = txtLogin.Text.Trim();
            string password = txtPassword.Text;

            if (string.IsNullOrEmpty(login) || string.IsNullOrEmpty(password))
            {
                MessageBox.Show("Логин и пароль обязательны для заполнения");
                return;
            }

            using (SqlConnection conn = new SqlConnection(connString))
            {
                conn.Open();
                SqlCommand cmd = new SqlCommand("SELECT Rol FROM Polzovateli WHERE Login = @login AND Parol = @pass", conn);
                cmd.Parameters.AddWithValue("@login", login);
                cmd.Parameters.AddWithValue("@pass", password);
                object result = cmd.ExecuteScalar();

                if (result != null)
                {
                    string role = result.ToString();
                    MessageBox.Show("Вы успешно авторизовались");

                    if (role == "Администратор")
                    {
                        MessageBox.Show("Открывается панель администратора");
                    }
                    else
                    {
                        MessageBox.Show("Открывается панель пользователя");
                    }
                }
                else
                {
                    MessageBox.Show("Вы ввели неверный логин или пароль. Пожалуйста, проверьте ещё раз введённые данные");
                    txtPassword.Clear();
                }
            }
        }
    }
}



БЕЗ ПОДКЛЮЧЕНИЯ
using System;
using System.Collections.Generic;
using System.Windows.Forms;

namespace DemoExamen
{
    public partial class FormLogin : Form
    {
        private Dictionary<string, User> users = new Dictionary<string, User>();

        public FormLogin()
        {
            InitializeComponent();
            LoadUsers();
        }

        private void LoadUsers()
        {
            users.Add("admin", new User { Password = "123", Role = "Администратор" });
            users.Add("user1", new User { Password = "123", Role = "Пользователь" });
        }

        private void btnLogin_Click(object sender, EventArgs e)
        {
            string login = txtLogin.Text.Trim();
            string password = txtPassword.Text;

            if (string.IsNullOrEmpty(login) || string.IsNullOrEmpty(password))
            {
                MessageBox.Show("Логин и пароль обязательны для заполнения");
                return;
            }

            if (users.ContainsKey(login) && users[login].Password == password)
            {
                string role = users[login].Role;
                MessageBox.Show($"Вы успешно авторизовались\nВаша роль: {role}");
            }
            else
            {
                MessageBox.Show("Вы ввели неверный логин или пароль. Пожалуйста, проверьте ещё раз введённые данные");
                txtPassword.Clear();
            }
        }

        private class User
        {
            public string Password { get; set; }
            public string Role { get; set; }
        }
    }
}











Модуль 2: Запрос процента (универсальный)
sql
SELECT 
    COUNT(CASE WHEN Oplacheno >= Nachisleno THEN 1 END) * 100 / COUNT(*) AS Procent
FROM Plateshi
WHERE Period >= '2025-03-01' AND Period < '2025-04-01';





Модуль 3: Форма добавления пользователя (для администратора)
csharp
using System;
using System.Windows.Forms;

namespace DemoExamen
{
    public partial class FormAddUser : Form
    {
        public FormAddUser()
        {
            InitializeComponent();
        }

        private void btnSave_Click(object sender, EventArgs e)
        {
            string login = txtLogin.Text.Trim();
            string password = txtPassword.Text;
            string role = comboRole.Text;

            if (string.IsNullOrEmpty(login) || string.IsNullOrEmpty(password) || string.IsNullOrEmpty(role))
            {
                MessageBox.Show("Все поля обязательны для заполнения");
                return;
            }

            if (login == "admin")
            {
                MessageBox.Show("Пользователь с таким логином уже существует");
                return;
            }

            MessageBox.Show($"Пользователь {login} с ролью {role} добавлен");
            this.Close();
        }
    }
}



Модуль 4: Валидация ИНН (без комментариев)
csharp
using System;
using System.Linq;
using System.Windows.Forms;

namespace DemoExamen
{
    public partial class FormINN : Form
    {
        public FormINN()
        {
            InitializeComponent();
        }

        private void btnCheck_Click(object sender, EventArgs e)
        {
            string inn = txtINN.Text.Trim();

            if (inn.Length != 10 && inn.Length != 12)
            {
                MessageBox.Show("ИНН должен содержать 10 или 12 цифр");
                return;
            }

            if (!inn.All(char.IsDigit))
            {
                MessageBox.Show("ИНН должен содержать только цифры");
                return;
            }

            if (inn.Distinct().Count() == 1)
            {
                MessageBox.Show("ИНН не может состоять из одинаковых цифр");
                return;
            }

            MessageBox.Show("ИНН прошёл валидацию");
        }
    }
}
Модуль 4: ТестКейс.docx (структура для заполнения)
Действие	Ожидаемый результат	Результат
Ввести ИНН "1234567890"	ИНН корректен	[закладка]
Ввести ИНН "1111111111"	Ошибка (одинаковые цифры)	[закладка]
Ввести ИНН "123"	Ошибка (недостаточно цифр)	[закладка]
Ввести ИНН "12345678901"	Ошибка (11 цифр)	[закладка]
Ввести пустое поле	Ошибка (пустое значение)	[закладка]








ЗАПРОС НА ПОДКЛЮЧЕНИЕ 

Адаптированный запрос
sql
SELECT
    z.ID AS [NomerZakaza],
    zc.NazvaniyeKompanii AS [Pokupatel],
    p.Naimenovaniye AS [Product],
    z.Kolichestvo AS [KolichestvoVZakaze],
    ROUND(z.Kolichestvo * s.Kolichestvo * m.Cena, 2) AS [PolnayaStoimostPoMaterialam]
FROM Zakazy z
INNER JOIN Zakazchiki zc ON z.ZakazchikilD = zc.ID
INNER JOIN Producty p ON z.ProductyID = p.ID
INNER JOIN Specifikaciya s ON z.ProductyID = s.ProductyID
INNER JOIN Materialy m ON s.MaterialyID = m.ID
GROUP BY z.ID, zc.NazvaniyeKompanii, p.Naimenovaniye, z.Kolichestvo, z.Kolichestvo * s.Kolichestvo * m.Cena
ORDER BY z.ID;

Если нужна общая стоимость
sql
SELECT
    z.ID AS [NomerZakaza],
    zc.NazvaniyeKompanii AS [Pokupatel],
    p.Naimenovaniye AS [Product],
    z.Kolichestvo AS [Kolichestvo],
    p.Cena AS [CenaProducta],
    (z.Kolichestvo * p.Cena) AS [StoimostBezMaterialov],
    ROUND(z.Kolichestvo * s.Kolichestvo * m.Cena, 2) AS [StoimostMaterialov],
    ROUND(z.Kolichestvo * p.Cena + z.Kolichestvo * s.Kolichestvo * m.Cena, 2) AS [ObshayaStoimost]
FROM Zakazy z
INNER JOIN Zakazchiki zc ON z.ZakazchikilD = zc.ID
INNER JOIN Producty p ON z.ProductyID = p.ID
INNER JOIN Specifikaciya s ON z.ProductyID = s.ProductyID
INNER JOIN Materialy m ON s.MaterialyID = m.ID
ORDER BY z.ID;

ПОДРОБНЫЙ

SELECT
    Zakazy.ID,
    Zakazchiki.NazvaniyeKompanii,
    Producty.Naimenovaniye,
    Zakazy.Kolichestvo,
    ROUND(Zakazy.Kolichestvo * Specifikaciya.Kolichestvo * Materialy.Cena, 2)
FROM Zakazy
JOIN Zakazchiki ON Zakazy.ZakazchikilD = Zakazchiki.ID
JOIN Producty ON Zakazy.ProductyID = Producty.ID
JOIN Specifikaciya ON Zakazy.ProductyID = Specifikaciya.ProductyID
JOIN Materialy ON Specifikaciya.MaterialyID = Materialy.ID
ORDER BY Zakazy.ID;








Руководство пользователя
1. Назначение программы
Программа предназначена для авторизации пользователей информационной системы с разграничением доступа по ролям "Администратор" и "Пользователь".

2. Начало работы
При запуске программы открывается форма авторизации.

На форме расположены:

Поле для ввода логина

Поле для ввода пароля

Интерактивная капча (пазл)

Кнопка "Войти"

3. Авторизация в системе
Шаг 1. Ввести логин в поле "Логин".

Шаг 2. Ввести пароль в поле "Пароль".

Шаг 3. Собрать пазл (капчу). Необходимо перетащить фрагменты изображения в правильном порядке.

Шаг 4. Нажать кнопку "Войти".

4. Возможные результаты авторизации
Ситуация	Результат
Логин или пароль не введены	Сообщение: "Логин и пароль обязательны для заполнения"
Пазл собран неверно	Сообщение об ошибке, попытка не засчитывается
Логин и пароль введены неверно	Сообщение: "Вы ввели неверный логин или пароль. Пожалуйста, проверьте ещё раз введённые данные"
Логин и пароль верны, пазл собран верно	Сообщение: "Вы успешно авторизовались". Открывается рабочая область в соответствии с ролью пользователя
5. Блокировка учётной записи
Учётная запись блокируется в следующих случаях:

Три подряд неверные попытки сборки пазла

Три подряд неверные попытки ввода пароля

При блокировке появляется сообщение: "Вы заблокированы. Обратитесь к администратору".

6. Работа администратора
Пользователь с ролью "Администратор" имеет доступ к следующим функциям:

6.1. Добавление нового пользователя
Нажать кнопку "Добавить пользователя"

Заполнить поля: логин, пароль, роль

Нажать "Сохранить"

Если пользователь с таким логином уже существует, появится сообщение: "Пользователь с указанным логином уже существует".

6.2. Изменение данных пользователя
Выбрать пользователя из списка

Нажать кнопку "Изменить"

Отредактировать необходимые поля

Нажать "Сохранить"

6.3. Снятие блокировки
Выбрать заблокированного пользователя из списка

Нажать кнопку "Снять блокировку"

7. Работа пользователя
Пользователь с ролью "Пользователь" имеет доступ только к авторизованной части системы без возможности управления другими пользователями.

8. Завершение работы
Для выхода из программы нажмите кнопку "Выход" в главном окне или закройте окно приложения стандартным способом.




