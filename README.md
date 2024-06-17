// ВПР 
        
    =ВПР(А2;$D$16;$E$18;2;0)
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
    
// Добавление в бд

    namespace Kod
    {
    /// <summary>
    /// Логика взаимодействия для ADSotr.xaml
    /// </summary>
    public partial class ADSotr : Window
    {
        private user _currentuser = new user();
        public ADSotr(user selecteduser)
        {
            InitializeComponent();
            if (selecteduser != null)
                _currentuser = selecteduser;
            DataContext = _currentuser;
        }

        private void Save_Click(object sender, RoutedEventArgs e)
        {
            StringBuilder errors = new StringBuilder();

            if (string.IsNullOrWhiteSpace(_currentuser.lastname))
                errors.AppendLine("Укажите фамилию");
            if (string.IsNullOrWhiteSpace(_currentuser.firstname))
                errors.AppendLine("Укажите имя");
            if (string.IsNullOrWhiteSpace(_currentuser.middlename))
                errors.AppendLine("Укажите отчество");
            if (string.IsNullOrWhiteSpace(_currentuser.status))
                errors.AppendLine("Укажите статус");

            if (errors.Length > 0)
            {
                MessageBox.Show(errors.ToString());
                return;
            }

            if (_currentuser.userid == 0)
               name_databaseEntities.GetContext().user.Add(_currentuser);

            try
            {
                name_databaseEntities.GetContext().SaveChanges();
                //MessageBox.Show("Информация сохранена!");

            }
            catch (Exception ex)
            {
                MessageBox.Show(ex.Message.ToString());
            }


            this.Close();
        }
    }
    }
    <Grid>
        <TextBox HorizontalAlignment="Left" Height="23" Margin="263,118,0,0" TextWrapping="Wrap" Text ="{Binding lastname}" VerticalAlignment="Top" Width="120"/>
        <TextBox HorizontalAlignment="Left" Height="23" Margin="263,170,0,0" TextWrapping="Wrap" Text="{Binding firstname}" VerticalAlignment="Top" Width="120"/>
        <TextBox HorizontalAlignment="Left" Height="23" Margin="263,212,0,0" TextWrapping="Wrap" Text="{Binding middlename}" VerticalAlignment="Top" Width="120"/>
        <TextBox HorizontalAlignment="Left" Height="23" Margin="263,255,0,0" TextWrapping="Wrap" Text="{Binding status}" VerticalAlignment="Top" Width="120"/>
        <Button Content="Сохранить" Name="Save" Click="Save_Click" HorizontalAlignment="Left" Margin="263,326,0,0" VerticalAlignment="Top" Width="75"/>

    </Grid>

// XAML Master 

    <Window x:Class="Kod.Master"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:Kod"
        mc:Ignorable="d"
        Title="Master" Height="450" Width="800">
    <Grid>
        <Button Name="Sotrud" Click="Sotrud_Click" Content="Сотрудники" HorizontalAlignment="Left" Margin="10,10,0,0" VerticalAlignment="Top" Width="75"/>
        <Button Name="Zakaz" Click="Zakaz_Click" Content="Заказы" HorizontalAlignment="Left" Margin="10,43,0,0" VerticalAlignment="Top" Width="75"/>
        <Button Name="Smena" Click="Smena_Click" Content="Смена" HorizontalAlignment="Left" Margin="10,78,0,0" VerticalAlignment="Top" Width="75"/>
        <DockPanel Height="450" Margin="97,0,-0.4,-30">
            <Border BorderBrush="Black" BorderThickness="2" DockPanel.Dock="Top" >
                <StackPanel Margin="-1.6,-2,-1.8,-2">
                    <Frame Name="MyFrame" Margin="-1.6,-2,-1.8,-2">
                        
                    </Frame>
                </StackPanel>
            </Border>
        </DockPanel>
    </Grid>
    </Window>
    public partial class Master : Window
    {
        public Master()
        {
            InitializeComponent();
        }

        private void Sotrud_Click(object sender, RoutedEventArgs e)
        {
            MyFrame.Content = new Sotrudniki();
        }

        private void Zakaz_Click(object sender, RoutedEventArgs e)
        {
            MyFrame.Content = new Zakaz();
        }

        private void Smena_Click(object sender, RoutedEventArgs e)
        {
            MyFrame.Content = new Smena();
        }
    }
    }
