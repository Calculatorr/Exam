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
Руководство пользователя

        Руководство пользователя
        программного обеспечения
        «АЛЗАМИР-Контакт»
        Кемерово
        2023 год
        2
        ОГЛАВЛЕНИЕ
        1. ВВЕДЕНИЕ.............................................................................................................3
        2. НАЗНАЧЕНИЕ........................................................................................................3
        3. ПОДГОТОВКА К РАБОТЕ .....................................................................................3
        4. РАБОТА С ПО........................................................................................................4
        4.1. Вход в ПО...............................................................................................................4
        4.2. Главная страница ..................................................................................................5
        4.3. Выбор подсистем...................................................................................................7
        4.4. График....................................................................................................................8
        4.5. События................................................................................................................10
        4.6. Отчеты..................................................................................................................11
        4.7. Сервисный режим................................................................................................13
        4.8. Режим проверки...................................................................................................14
        4.9. Строка событий....................................................................................................16
        4.10. Сообщения........................................................................................................17
        5. ТРЕБОВАНИЯ ПО ...............................................................................................19
        5.1. Требования квалификации персонала...............................................................19
        5.2. Требования к АРМ ...............................................................................................19
        Руководство пользователя программного обеспечения «АЛЗАМИР-Контакт»
        1. ВВЕДЕНИЕ
        Настоящее руководство пользователя распространяется на программное 
        обеспечение (далее - ПО) «АЛЗАМИР-Контакт» версии 1.0. Руководство пользователя 
        содержит технические характеристики, описание программного обеспечения, принципы 
        взаимодействия пользователя с программным обеспечением и указания, необходимые для 
        правильной и безопасной его эксплуатации.
        2. НАЗНАЧЕНИЕ
        Программное обеспечение «АЛЗАМИР-Контакт» предназначено для сбора данных 
        с систем безопасности входящих в состав многофункциональной системы безопасности
        (далее - МФСБ), а также дополнительных систем предназначенных для обеспечения 
        безопасного ведения производственных работ опасного производственного объекта, с 
        функцией передачи сообщений в Ростехнадзор и должностным лицам предприятий о 
        нарушениях ПБ, авариях и инцидентах.
        Программное обеспечение представляет собой WEB-ориентированную платформу, 
        состоящую из графической части (веб-приложение), и серверной части. В серверную часть 
        входят функции хранения и обработки информации, составление отчетов. Отчеты 
        формируются в форматах pdf, doc, xls. Клиентская часть является веб-сайтом, 
        визуализирующим функционал серверной части.
        3. ПОДГОТОВКА К РАБОТЕ
        Для начала работы с ПО «АЛЗАМИР-Контакт» на автоматизированном рабочем 
        месте (далее - АРМ) диспетчера МФСБ должен быть предустановлен один из 
        предложенных WEB – браузеров: Google Chrome, Microsoft Edge, Mozilla Firefox, Opera, с 
        уставленными обновлениями не позднее 2020 года. 
        3
        Руководство пользователя программного обеспечения «АЛЗАМИР-Контакт»
        4
        4. РАБОТА С ПО
        4.1. Вход в ПО
        Для входа в ПО «АЛЗАМИР-Контакт» необходимо в адресной строке браузера
        ввести IP адрес или доменное имя сервера МФСБ как указанно на (рис. 4.1.1) IP адрес или
        доменное имя сервера МФСБ предоставляется Администратором МФСБ.
        Рисунок 4.1.1 – Ввод IP адреса
        При правильном введении IP адреса или доменного имени сервера МФСБ вы
        попадаете на страницу авторизации ПО «АЛЗАМИР-Контакт» рис. 4.1.2.
        Рисунок 4.1.2 – Авторизация пользователя
        Для получения доступа к данным необходимо выполнить авторизацию: указать
        логин и пароль (выдаётся администратором МФСБ), а затем нажать кнопку “Войти”.
        Руководство пользователя программного обеспечения «АЛЗАМИР-Контакт»
        5
        Кнопка “Запомнить меня” при активации позволяет сохранить авторизацию
        пользователя при следующем запуске системы. Если данная кнопка не активна, то при
        следующем запуске, пользователю снова нужно пройти авторизацию.
        4.2. Главная страница
        После успешной авторизации в ПО вы попадаете на главную страницу МФСБ (рис.
        4.2.1).
        Рисунок 4.2.1 – Главная страница
        На главной странице ПО «АЛЗАМИР-Контакт» представлен список систем
        безопасности и их состояние в режиме реального времени, строка событий (см. раздел
        4.9.). Цветовая индикация отображает текущее состояние системы как в целом так и
        отдельных её компонентов. В нижней части экрана (рис. 4.2.2) представлена цветовая
        кодировка состояния систем.
        Рисунок 4.2.2 – Цветовая кодировка
        В верхней части экрана отображается текущее предприятие и данные
        пользователя авторизованного в системе.
        Руководство пользователя программного обеспечения «АЛЗАМИР-Контакт»
        6
        Для того чтобы сменить организацию, пользователю необходимо:
        1. В верхнем меню нажать на название текущей организации;
        2. В появившемся выпадающем меню кликнуть по нужной организации (рис.
        4.2.1).
        После выполненных действий список систем на экране смениться, а в верхнем
        меню будет отображаться имя выбранной организации.
        Для настройки яркости отображения систем в левом верхнем углу нажмите значок
        изменения яркости (рис. 4.2.3). Передвигая появившийся ползунок влево или вправо
        изменяется яркость отображения систем в большую или меньшую сторону.
        Рисунок 4.2.3 – Изменение яркости, выход из ПО
        Выход из ПО осуществляется путем нажатия на копку выхода (рис. 4.2.3) в правом
        верхнем углу главной страницы.
        Для просмотра событий нажмите на кнопку «События» и вы перейдете в меню
        отображения событий (см. раздел 4.5).
        Для построения отчетов нажмите на кнопку «Отчеты» и вы перейдете в меню
        построения отчетов (см. раздел 4.6.).
        Руководство пользователя программного обеспечения «АЛЗАМИР-Контакт»
        7
        4.3. Выбор подсистем
        На главной странице ПО отображаются системы и подсистемы входящие в их
        состав. Для того чтобы перейти на страницу подсистемы пользователю необходимо
        нажать на интересующую его подсистему.
        На открывшейся странице подсистемы (рис. 4.3.1) отображаться список датчиков
        входящих в состав данной подсистемы, их состояние и показания измеряемых параметров
        в режиме реального времени.
        Рисунок 4.3.1 – Страница отображения подсистемы
        Для детального просмотра состояния датчика необходимо нажать на
        интересующий вас датчик, после чего откроется окно детальной информации о состоянии
        датчика (рис. 4.3.2). В открывшемся окне отображаться статус оборудования, его
        состояние, тип датчика, номер датчика и текущие показания.
        В окне состояния датчика, возможно, построить отчет по события (см. раздел 4.5.),
        посмотреть график с показаниями за заданный период (см. раздел 4.4.), перевести в
        режим проверки (см. раздел 4.6.) или сервисный режим (см. раздел 4.7.) и выполнить
        отправку сообщения (см. раздел 4.10.).
        Скрытие окна состояния датчика осуществляется путем нажатия на подсистему.
        Для выхода из окна подсистемы на предыдущую страницу нажмите на систему в
        левом верхнем углу страницы.
        Для выхода из окна подсистемы на главную страницу необходимо нажать на
        кнопку «Системы МФСБ» в левом верхнем углу страницы.
        Руководство пользователя программного обеспечения «АЛЗАМИР-Контакт»
        8
        Рисунок 4.3.2 – Окно состояния датчика
        4.4. График
        Для построения графика необходимо выбрать интересующий вас датчик, и нажать
        на иконку датчика для вызова окна состояния датчика (см. радел 4.3.). В открывшемся окне
        необходимо нажать на кнопку «График». В открывшемся окне графика выбранного датчика
        (рис. 4.4.1) отображается значение датчика за заданный период времени. Чтобы изменить
        период времени необходимо установить начало и конец интересующего вас периода, для
        этого необходимо нажать на дату периода, после чего откроется календарь (рис. 4.4.2), где
        можно указать конкретный период. Для более точного указания периода возможно указать
        точность до секунд за интересующий вас период. Что бы выбрать точность периода
        времени до секунд нажмите на открывшемся календаре на значок часов (рис. 4.4.2), после
        укажите время и нажмите кнопку принять.
        Руководство пользователя программного обеспечения «АЛЗАМИР-Контакт»
        Рисунок 4.4.1 – График датчика
        В окне графика датчика, возможно, развернуть график на всю страницу, для этого
        нажмите на значок развёртывания графика расположенного в правом верхнем углу (рис.
        4.4.1).Для того чтобы свернуть окно графика в прежнее состояние, нажмите повторно
        развёртывания графика расположенного в правом верхнем углу.
        Для масштабирования текущего графика наведите указатель мыши на график и
        поворотом колесика мыши изменяется масштаб в большую или меньшую сторону.
        Для осуществления навигации по графику наведите указатель мышки на график и
        зажав левую кнопу мыши, перемешайте мышь в левую или правую сторону и вверх, вниз.
        Таким образом, будет осуществляться навигация в выбранном масштабе и периоде
        времени.
        9
        Руководство пользователя программного обеспечения «АЛЗАМИР-Контакт»
        10
        Рисунок 4.4.2 – Выбор периода времени
        4.5. События
        Для перехода к окну событий, необходимо на главной странице нажать на кнопку с
        надписью «События», после чего откроется окно событий (рис. 4.5.1)
        Рисунок 4.5.1 – Окно событий
        Руководство пользователя программного обеспечения «АЛЗАМИР-Контакт»
        11
        Чтобы просмотреть события необходимо установить начало и конец
        интересующего вас периода, для этого необходимо нажать на дату периода, после чего
        откроется календарь, где можно указать конкретный период. Для более точного указания
        периода возможно указать точность до секунд за интересующий вас период. Чтобы
        выбрать точность периода времени до секунд нажмите на открывшемся календаре на
        значок часов, после укажите время и нажмите кнопку принять. После установки периода
        нажать на кнопку «Посмотреть события».
        Сформированные события отображаться в виде таблицы, где указаны следующие
        параметры: номер события, система, датчик, уровень сигнала, дата и время начала и
        окончания, комментарий. В сформированном отчете возможна сортировка по
        перечисленным параметрам, для этого выберете интересующий вас параметр и нажмите
        левой клавишей мыши, после чего в первую очередь будут отображаться события по
        выбранному параметру.
        Формирование событий возможно для отдельной системы, подсистемы, датчика и
        типу сигнала. Для этого выберите интересующий вас тип системы, подсистемы, датчика и
        тип сигнала, после чего нажмите на кнопку «Посмотреть события».
        4.6. Отчеты
        Для перехода к окну событий, необходимо на главной странице нажать на кнопку с
        надписью «Отчеты», после чего откроется окно отчетов (рис. 4.6.1).
        Рисунок 4.6.1 – Окно отчетов
        Руководство пользователя программного обеспечения «АЛЗАМИР-Контакт»
        Что бы сформировать отчет необходимо установить начало и конец
        интересующего вас периода, для этого необходимо нажать на дату периода, после чего
        откроется календарь, где можно указать конкретный период. Для более точного указания
        периода возможно указать точность до секунд за интересующий вас период. Что бы
        выбрать точность периода времени до секунд нажмите на открывшемся календаре на
        значок часов, после укажите время и нажмите кнопку принять. После установки периода
        выбрать формат отчёта и нажать на кнопку формирования отчета (PDF, XLS, WORD).
        Далее сформированный отчет (рис. 4.6.2) автоматически сохраняется в папку «Загрузки»
        (уславливается по умолчанию или другое место заданное администратором МФСБ).
        Рисунок 4.6.2 – Загрузка отчета
        Сформированный отчет отображается в виде таблицы (рис. 4.6.3), где указаны
        следующие параметры: номер события, система, датчик, уровень сигнала, дата и время
        начала и окончания, комментарий.
        12
        Руководство пользователя программного обеспечения «АЛЗАМИР-Контакт»
        13
        Рисунок 4.6.3 – Сформированный отчет
        Формирование отчетов возможно для отдельной системы, подсистемы, датчика и
        типу сигнала. Для этого выберите интересующий вас тип системы, подсистемы, датчика и
        тип сигнала, после чего необходимо выбрать формат отчёта и нажать на кнопку
        формирования отчета (PDF, XLS, WORD).
        4.7. Сервисный режим
        На главной странице в поле системы, для которой необходимо ввести сервисный
        режим нажмите на кнопку перевода системы в сервисный режим (рис. 4.7.1), после чего
        появиться меню выбора режима работы системы и на жмите на кнопку «Перевести в
        сервисный режим».
        Рисунок 4.7.1 – Выбор режима системы
        Далее в окне (рис. 4.7.2) подтверждения перевода системы в сервисный режим в
        поле комментарий необходимо указать причину, по которой система переведена в
        сервисный режим.
        Руководство пользователя программного обеспечения «АЛЗАМИР-Контакт»
        14
        Рисунок 4.7.2 – Окно подтверждения перевода системы в сервисный режим
        После успешного перевода системы в сервисный режим система отображается как
        указанно на рис. 4.7.3.
        Рисунок 4.7.3 – Система в сервисном режиме
        ВНИМАНИЕ!!!
        При работе системы в сервисном режиме не формируются события и не
        производиться запись в журнал событий, не производиться информирование о
        превышении критических значений системы. Перевод системы в сервисный режим должен
        осуществляется в период проведения плановых профилактических работ с последующим
        выходом системы из сервисного режима по окончанию периода проведения плановых
        профилактических работ.
        Перевод в сервисный режим работы возможен как для систем, отдельных
        подсистем, отдельных датчиков (см. раздел 4.3.).
        4.8. Режим проверки
        На главной странице в поле системы, для которой необходимо ввести режим
        проверки нажмите на кнопку перевода системы в режим проверки (рис. 4.8.1) , после чего
        Руководство пользователя программного обеспечения «АЛЗАМИР-Контакт»
        15
        появиться меню выбора режима работы системы и нажмите на кнопку «Перевести в режим
        проверки».
        Рисунок 4.8.1 – Выбор режима системы
        Далее в окне (рис. 4.8.2) подтверждения перевода системы в режим проверки в
        поле комментарий необходимо указать причину, по которой система переведена в режим
        проверки.
        Рисунок 4.8.2 – Окно подтверждения перевода системы в режим проверки
        После успешного перевода системы в сервисный режим система отображается как
        указанно на рис. 4.8.3.
        Рисунок 4.8.3 – Система в режиме проверки
        ВНИМАНИЕ!!!
        При работе системы в режиме проверки не формируются события, не
        производиться информирование о превышении критических значений системы. Перевод
        системы в режим проверки должен осуществляется в период проведения плановой
        Руководство пользователя программного обеспечения «АЛЗАМИР-Контакт»
        16
        проверки системы с последующим выходом системы из режима проверки по окончанию
        периода планового проведения проверки системы.
        Перевод в режим проверки возможен как для систем, отдельных подсистем,
        отдельных датчиков (см. раздел 4.3.).
        4.9. Строка событий
        На главном экране в нижней части находиться строка событий (рис. 4.9.1), в данной
        строке отображаться события которые произошли в системах, подсистемах, датчиков.
        Рисунок 4.9.1 – Строка событий
        Для квитирования события нажмите на интересующее событие, в открывшемся
        окне (рис. 4.9.2) отображается информация о событии. Нажав на кнопку «Перейти к
        объекту» автоматически происходит применение на страницу данного объекта. Для
        квитирования события необходимо выбрать один из предложенных вариантов и ввести
        комментарий, после чего данное событие исчезнет из строки событий.
        Рисунок 4.9.2 – Квитирование событий
        Руководство пользователя программного обеспечения «АЛЗАМИР-Контакт»
        Квитирование события как «не устранено» не требует обязательного ввода
        комментария, и событие остаётся в строке событий.
        Указанные комментарии при квитировании событий записываться в события и
        отчеты в поле «Комментарий».
        4.10. Сообщения
        Для оправки сообщений ручным способом необходимо на главном экране выбрать
        подсистему, с которой необходимо осуществить отправку сообщения в ручном режиме.
        Далее в открывшемся окне подсистемы в правом верхнем углу (рис. 4.10.1) нажать на знак
        конверта.
        Рисунок 4.10.1 – Отправка сообщений
        В открывшемся меню (рис. 4.10.2) отправки сообщений из списка получателей
        выбрать Email должностных лиц, для которых необходима отправка сообщения и ввести
        текст сообщения, после чего нажать кнопку «Отправить». Указанным лицам на Email
        придёт электронное сообщение с информацией о подсистеме, с которой осуществлялась
        отправка сообщения, о состоянии датчиков этой подсистемы, указанным комментарием.
        17
        Руководство пользователя программного обеспечения «АЛЗАМИР-Контакт»
        Рисунок 4.10.2 – Меню отправки сообщений
        18
        Руководство пользователя программного обеспечения «АЛЗАМИР-Контакт»
        19
        5. ТРЕБОВАНИЯ ПО
        5.1. Требования квалификации персонала
        Для обеспечения ввода программного обеспечения в действие персонал, должен 
        изучить эксплуатационную документацию на ПО «АЛЗАМИР-Контакт» и овладеть 
        навыками работы с ПО.
        Пользователи системы должны владеть навыками работы с Интернет-браузерами 
        (Google Chrome, Microsoft Edge, Mozilla Firefox, Opera).
        Сотрудники предприятия, использующие ПО в своей деятельности, проходят 
        обучение навыкам работы с ПО.
        5.2. Требования к АРМ
        Для обработки и анализа данных, получаемых с многофункциональной системы 
        безопасности горнодобывающих и горно-обогатительных предприятий необходимо 
        определить рабочие места пользователей ПО «АЛЗАМИР-Контакт».
        АРМ должно быть с предустановленной операционной системой (Window или
        Linux), установлен интернет-браузер (Google Chrome, Microsoft Edge, Mozilla Firefox, 
        Opera), средства работы с файлами формата: pdf, doc, xls.
        Технические требования к АРМ:
        Процессор: 1 гигагерц (ГГц) или быстрее с двумя или более ядрами на 
        совместимом 64-разрядном процессоре;
        ОЗУ: 4 гигабайта (ГБ) или больше;
        ПЗУ: 1 ГБ или больше свободного места на диске;
        Графическая карта: совместим с DirectX 12 или более поздней версии с драйвером 
        WDDM 2.0;
        Дисплей: дисплей с высоким разрешением (720p), монитор с разрешением 9 
        дюймов или более, 8 бит на цветовый канал;
        Устройства ввода: клавиатура, компьютерная мышь.
