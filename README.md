// Авторизация
public partial class MainWindow : Window
    {
        string login1 = "1";/*Менеджер*/
        string password1 = "1";
        string login2 = "2";
        string password2 = "2";
        public MainWindow()
        {
            InitializeComponent();
        }

        public int test_m(int a, int b)
        {
            return a + b;
        }

        private void Button_Click(object sender, RoutedEventArgs e)
        {
            if ((Login.Text == login1) && (Pasword.Text == password1))
            {
                var Window1GLAVNdliaMEN = new AutoSalon.GlavOkno();

                Window1GLAVNdliaMEN.Show();
            }
            else
            if ((Login.Text == login2) && (Pasword.Text == password2))
            {
                var Window1GLAVNdliaTEHNIK = new AutoSalon.GlavOkno();
                Window1GLAVNdliaTEHNIK.Show();
            }
            else
            {
                MessageBox.Show("Введен не верный логин или пароль!", "Ошибка", MessageBoxButton.OK, MessageBoxImage.Warning);
            }
            this.Close();
        }

     

        private void Login_MouseDoubleClick(object sender, MouseButtonEventArgs e)
        {
            Login.Text = "";
        }

        private void Pasword_MouseDoubleClick(object sender, MouseButtonEventArgs e)
        {
            Pasword.Text = "";
        }

        private void Login_TextChanged(object sender, TextChangedEventArgs e)
        {

        }
    }
}

// Подключение БД
public partial class AutoSalonEntities5 : DbContext
    {
        private static AutoSalonEntities5 _context;
        public AutoSalonEntities5()
            : base("name=AutoSalonEntities5")
        {
        }

        public static AutoSalonEntities5 GetContext()
        {
            if (_context == null)
                _context = new AutoSalonEntities5();
            return _context;
        }


// Вывод в грид
{
        public Page1()
        {
            InitializeComponent();
           
        }

        private void Page_IsVisibleChanged(object sender, DependencyPropertyChangedEventArgs e)
        {
            if (Visibility == Visibility.Visible)
            {
                AutoSalonEntities5.GetContext().ChangeTracker.Entries().ToList().ForEach(p => p.Reload());
                DGridPocup.ItemsSource = AutoSalonEntities5.GetContext().Pokupatel.ToList();
            }
        }

        private void adpo_Click(object sender, RoutedEventArgs e)
        {
            AddPokupatel addPokupatel = new AddPokupatel(null);
            addPokupatel.Show();
        }

        private void btndelp_Click(object sender, RoutedEventArgs e)
        {
            var clientForRemoving = DGridPocup.SelectedItems.Cast<Pokupatel>().ToList(); //переменная для удаления
            if (MessageBox.Show($"Вы уверены что хотите удалить {clientForRemoving.Count()} элемент(a/ов)?", "Внимание", MessageBoxButton.YesNo, MessageBoxImage.Warning) == MessageBoxResult.Yes)
            {

                try
                {
                    AutoSalonEntities5.GetContext().Pokupatel.RemoveRange(clientForRemoving);
                    AutoSalonEntities5.GetContext().SaveChanges();
                    MessageBox.Show("удалено");
                    DGridPocup.ItemsSource = AutoSalonEntities5.GetContext().Pokupatel.ToList();
                }
                catch (Exception ex) { MessageBox.Show(ex.Message.ToString()); }

            }
        }
    }
}

// Сохранение в БД
  public partial class AddPokupatel : Window
    {
        private Pokupatel _currentPokupatel = new Pokupatel();
        public AddPokupatel(Pokupatel selectedPokupatel)
        {
            InitializeComponent();

            if (selectedPokupatel != null)
                _currentPokupatel = selectedPokupatel;
            DataContext = _currentPokupatel;
        }

        private void BtnSave_Click(object sender, RoutedEventArgs e)
        {
            StringBuilder errors = new StringBuilder();

            if (string.IsNullOrWhiteSpace(_currentPokupatel.Familia))
                errors.AppendLine("Укажите фамилию");
            if (string.IsNullOrWhiteSpace(_currentPokupatel.Name))
                errors.AppendLine("Укажите имя");
            if (string.IsNullOrWhiteSpace(_currentPokupatel.Otchestvo))
                errors.AppendLine("Укажите отчество");
            if (string.IsNullOrWhiteSpace(_currentPokupatel.Pasport))
                errors.AppendLine("Укажите паспорт");

            if (errors.Length > 0)
            {
                MessageBox.Show(errors.ToString());
                return;
            }

            if (_currentPokupatel.Id_Buyer == 0)
                AutoSalonEntities5.GetContext().Pokupatel.Add(_currentPokupatel);

            try
            {
                AutoSalonEntities5.GetContext().SaveChanges();
                MessageBox.Show("Информация сохранена!");
                
            }
            catch (Exception ex)
            {
                MessageBox.Show(ex.Message.ToString());
            }


            
        }
    }
    }
    
