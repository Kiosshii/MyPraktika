# MyPraktika
123
[README.md](https://github.com/user-attachments/files/23756476/README.md)
## PomogSlonyare

Двуxкомпонентная система технической поддержки:

- **PomogSlonyare Admin** — WinForms‑приложение администратора (.NET 8, x86) с CRUD по пользователям и заявкам.
- **PomogSlonyare Client** — отдельное WinForms‑приложение для клиентов (регистрация, авторизация, собственные заявки).
- **Access/Jet база** — файл `PomogSlonyareData.mdb`, подключаемый через `System.Data.OleDb`. Таблицы и тестовые данные создаются автоматически при первом запуске.

### Стек и структура

```
PomogSlonyare.Admin   # Панель администратора
PomogSlonyare.Client  # Клиентское приложение
PomogSlonyare.Shared  # Общие модели + Access-репозиторий
PomogSlonyareData.mdb # Шаблон БД с начальными записями
PomogSlonyare.sln     # Общее решение
```

### Требования

- Windows + .NET SDK 8.0 (или Visual Studio 2022 17.8+).
- Провайдер Microsoft Jet/ACE OLE DB. Проще всего установить пакет [Access Database Engine 2016 x86](https://www.microsoft.com/en-us/download/details.aspx?id=54920) (подойдёт и 2010).
- Конфигурация сборки **x86** (прописано в `.csproj` по умолчанию), чтобы Jet 4.0 работал на 64‑битных системах.

### Как запустить

1. Откройте `PomogSlonyare.sln` в Visual Studio (рабочая нагрузка **.NET Desktop Development**) или выполните:
   ```powershell
   dotnet build
   dotnet run --project PomogSlonyare.Client
   dotnet run --project PomogSlonyare.Admin
   ```
2. При первом запуске в `%LOCALAPPDATA%\PomogSlonyare` копируется шаблон `PomogSlonyareData.mdb`, создаются таблицы `Users`, `Tickets`, а также предзаполненные записи:
   - Админ: `admin@pomogslonyare.local / admin123`.
   - Пользователи: Алексей и Мария с готовыми заявками («Не работает почта», «Ошибка авторизации», «Нужен доступ к VPN»), поэтому интерфейсы сразу заполнены.
3. Приложения можно запускать независимо:
   - Админ запускает `PomogSlonyare.Admin.exe` и управляет пользователями/тикетами.
   - Клиент запускает `PomogSlonyare.Client.exe`, регистрируется и отправляет заявки.

### Visual Studio

Чтобы стартовать всё одним нажатием:

1. В обозревателе решений выберите Solution → *Set Startup Projects…*.
2. Переключитесь на **Multiple startup projects** и выставьте `Start` для `PomogSlonyare.Client` и `PomogSlonyare.Admin`.
3. Нажмите `F5`/`Ctrl+F5` — откроются обе программы, работающие с общей Access‑базой.

### Обработка ошибок

- Все обращения к базе идут через общий репозиторий `PomogSlonyareRepository`, который:
  - автоматически создаёт таблицы и сидированные данные;
  - бросает осмысленные исключения при ошибках БД.
- WinForms интерфейсы оборачивают операции в `try/catch`, показывают диалог с текстом ошибки и блокируют повторный ввод, пока запрос не завершится.

### Полезные советы

- Перенос БД — просто скопируйте `%LOCALAPPDATA%\PomogSlonyare\PomogSlonyareData.mdb`.
- Для смены цветовой гаммы правьте только формы — бизнес‑логика и доступ к данным изолированы в `PomogSlonyare.Shared`.
- Хранение паролей упрощено (plain text для демонстрации); при желании можно заменить на SHA/BCrypt, изменив единственный репозиторий.


