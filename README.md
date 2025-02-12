Българска версия:

name: Selenium WebDriver CI  # Име на CI/CD работния процес (workflow), което ще се показва в GitHub Actions.

on:  # Дефинира кога да се стартира този workflow.
  push:  # Изпълнява се при push събитие.
    branches: [ "master" ]  # Само ако промените са в основния (master) клон.
  pull_request:  # Изпълнява се при създаване на pull request.
    branches: [ "master" ]  # Отново само за основния (master) клон.

jobs:  # Дефиниция на задачи (jobs), които ще се изпълнят.
  build:  # Името на тази задача (job).

    runs-on: ubuntu-latest  # Изпълнява се в последната налична версия на Ubuntu.

    steps:  # Списък със стъпките, които ще се изпълнят.
    - name: Checkout repository to the runner  # Изтегля кода от хранилището.
      uses: actions/checkout@v4  # GitHub Action за клониране на репото в runner-а.

    - name: Setup .NET Core  # Инсталира .NET SDK.
      uses: actions/setup-dotnet@v4  # GitHub Action за настройка на .NET средата.
      with:
        dotnet-version: 6.0.x  # Определя, че ще се използва .NET 6.0.

    - name: Install Chrome  # Инсталира Google Chrome в runner-а.
      run: |
         sudo apt-get update  # Обновява списъка с пакети.
         sudo apt-get install -y google-chrome-stable  Инсталира Chrome.

    - name: Install dependencies  # Инсталира зависимостите на проекта.
      run: dotnet restore SeleniumBasicExercise.sin  # Възстановява всички пакети, нужни за компилация.

    - name: Build  # Компилира проекта.
      run: dotnet build SeleniumBasicExercise.sin --no-restore  
      # --no-restore означава, че няма да се изпълнява `dotnet restore` отново.

    - name: Run TestProject1 tests  # Стартира тестовете на TestProject1.
      env:  # Дефинира променливите на средата.
        CHROMEWEBDRIVER: /usr/bim/google-chrome  # Указва пътя към Chrome WebDriver `/usr/bin`).
      run: |
       echo "Running TestProject1 tests"  # Извежда съобщение в конзолата.
       dotnet test TestProject1/TestProject1.csproj --no-build --verbosity normal  
       # --no-build означава, че тестовете ще се стартират без повторна компилация.

    - name: Run TestProject2 tests  # Стартира тестовете на TestProject2.
      env: 
        CHROMEWEBDRIVER: /usr/bim/google-chrome  # ГРЕШКА: `/usr/bim` -> `/usr/bin`.
      run: |
       echo "Running TestProject2 tests"
       dotnet test TestProject2/TestProject2.csproj --no-build --verbosity normal  

    - name: Run TestProject3 tests  # Стартира тестовете на TestProject3.
      env: 
        CHROMEWEBDRIVER: /usr/bim/google-chrome  # ГРЕШКА: `/usr/bim` -> `/usr/bin`.
      run: |
       echo "Running TestProject3 tests"
       dotnet test TestProject3/TestProject3.csproj --no-build --verbosity normal  


📌 YAML кодът:
yaml

name: Selenium WebDriver CI
Какво е това?
Определя името на CI/CD workflow-а в GitHub Actions.
Защо се използва?
За да се вижда името в GitHub Actions, когато workflow-ът се изпълнява.
Свързано с:

Целия workflow.
yaml

on:
  push:
    branches: [ "master" ]
  pull_request:
    branches: [ "master" ]

Какво е това?
Определя кога да се стартира workflow-ът.
Какво прави?
Автоматично стартира тестовете при:
push (нов код е качен в master клона).

pull request (някой предлага промени за master).
Свързано с:
Контролира кога се изпълняват стъпките на workflow-а.
yaml

jobs:
  build:
Какво е това?
Дефинира една "работа" (job), наречена build.
Защо се използва?
Всички стъпки за билдване и тестване ще бъдат в този job.
Свързано с:
Включва всички стъпки на CI/CD процеса.

yaml
runs-on: ubuntu-latest
Какво е това?
Определя коя операционна система ще се използва за изпълнение на тестовете.
Какво прави?
Използва последната версия на Ubuntu като среда за изпълнение.
Свързано с:
Всички следващи стъпки се изпълняват в тази среда.
    
    
yaml
    steps:
Какво е това?
Дефинира стъпките, които ще бъдат изпълнени в job-а.

yaml
    - name: Checkout repository to the runner
      uses: actions/checkout@v4
Какво е това?
Копира (checkout-ва) репото от GitHub на runner-а (машината, където се изпълнява job-а).
Защо се използва?
За да можем да използваме кода локално в CI/CD.
Свързано с:
Следващите стъпки, които работят с кода.

yaml
    - name: Setup .NET Core
      uses: actions/setup-dotnet@v4
      with:
        dotnet-version: 6.0.x
Какво е това?
Инсталира .NET Core 6.0.x на CI/CD runner-а.
Защо се използва?
Selenium тестовете са написани на C# и изискват .NET Core.
Свързано с:

dotnet restore, dotnet build и dotnet test.
yaml
    - name: Install Chrome
      run: |
         sudo apt-get update
         sudo apt-get install -y google-chrome-stable
Какво е това?
Инсталира Google Chrome на runner-а.
Защо се използва?
Selenium WebDriver изисква браузър, за да изпълнява тестове.
Свързано с:

Chrome WebDriver и тестовете.
yaml
    - name: Install Chrome WebDriver
      run: |
         sudo apt-get install -y chromium-chromedriver
Какво е това?
Инсталира Chrome WebDriver, който е нужен за автоматизираното управление на Chrome.
Защо се използва?
Без WebDriver Selenium тестовете няма да могат да стартират Chrome.
Свързано с:

Selenium WebDriver.
yaml
Copy
Edit
    - name: Install dependencies
      run: dotnet restore SeleniumBasicExercise.sln
Какво е това?
Изтегля и инсталира зависимостите от .csproj файловете.
Защо се използва?
За да са налични всички библиотеки, нужни за изпълнението на кода.
Свързано с:
.NET проектите.

yaml
    - name: Build
      run: dotnet build SeleniumBasicExercise.sln --no-restore
Какво е това?
Компилира кода, без да инсталира отново зависимостите (--no-restore).
Защо се използва?
За да сме сигурни, че кодът се компилира успешно преди тестовете.
Свързано с:
.csproj и .sln файловете.

yaml

    - name: Run TestProject1 tests
      env: 
        CHROMEWEBDRIVER: /usr/lib/chromium-browser/chromedriver
      run: |
       echo "Running TestProject1 tests"
       dotnet test TestProject1/TestProject1.csproj --no-build --verbosity normal
Какво е това?
Изпълнява Selenium тестовете за TestProject1.
Защо се използва?
Проверява дали този проект работи правилно.
Свързано с:

Google Chrome и WebDriver.
yaml
    - name: Run TestProject2 tests
      env: 
        CHROMEWEBDRIVER: /usr/lib/chromium-browser/chromedriver
      run: |
       echo "Running TestProject2 tests"
       dotnet test TestProject2/TestProject2.csproj --no-build --verbosity normal
Какво е това?
Изпълнява тестовете за TestProject2.
Защо се използва?
Проверява дали този проект работи правилно.

yaml
    - name: Run TestProject3 tests
      env: 
        CHROMEWEBDRIVER: /usr/lib/chromium-browser/chromedriver
      run: |
       echo "Running TestProject3 tests"
       dotnet test TestProject3/TestProject3.csproj --no-build --verbosity normal
Какво е това?
Изпълнява тестовете за TestProject3.
Защо се използва?
Проверява дали този проект работи правилно.


📌 Таблица с обобщение
Команда	                                        Какво прави?                                  Защо се използва?            Свързано с?
actions/checkout@v4	                            Клонира код                                   За достъп до проекта	       Следващите стъпки
actions/setup-dotnet@v4	                        Инсталира .NET	                              За компилиране и тестове     .NET проекти
sudo apt-get install -y google-chrome-stable	  Инсталира Chrome                              Нужен за Selenium	           WebDriver
sudo apt-get install -y chromium-chromedriver	  Инсталира WebDriver                           Нужен за Selenium            Chrome
dotnet restore	                                Инсталира зависимостите	                      За да работи проектът        .csproj файлове
dotnet build	                                  Компилира кода                                За да се уверим, че работи   .sln файл
dotnet test	                                    Стартира тестовете	                          Проверява проекта	           Selenium
Така CI/CD процесът тества всичко автоматично! 🚀




Обяснение на термините env, sudo и apt-get
1️⃣ env – Какво е и за какво се използва?
📌 Какво е env?
env е променлива на средата (environment variable), която се използва за настройка на параметри, достъпни за скрипта или програмата.

📌 Какво прави?
Дефинира глобални променливи, които програмите могат да използват.
Позволява на тестовете и приложенията да намират нужните ресурси.
📌 Пример в кода:

yaml
    - name: Run TestProject1 tests
      env: 
        CHROMEWEBDRIVER: /usr/lib/chromium-browser/chromedriver
      run: |
       echo "Running TestProject1 tests"
       dotnet test TestProject1/TestProject1.csproj --no-build --verbosity normal
📌 Какво означава това?
Тук CHROMEWEBDRIVER е зададен като променлива на средата и сочи към пътя на Chrome WebDriver.
Когато Selenium стартира Chrome, то използва този WebDriver.


2️⃣ sudo – Какво е и за какво се използва?
📌 Какво е sudo?
sudo (SuperUser DO) е команда в Linux, която дава администраторски права за изпълнение на команди.
Използва се за инсталиране на софтуер, промяна на системни файлове и изпълнение на команди, които изискват root права.
📌 Какво прави?
Позволява на обикновен потребител да изпълнява административни команди.
Без sudo някои команди няма да работят, защото Linux не позволява на обикновените потребители да правят промени в системата.
📌 Пример в кода:
yaml
    - name: Install Chrome
      run: |
         sudo apt-get update
         sudo apt-get install -y google-chrome-stable
📌 Какво означава това?
sudo apt-get update
Обновява списъка с пакети (проверява за нови версии).
sudo apt-get install -y google-chrome-stable
Инсталира Google Chrome, като използва административни права.
⚠️ Без sudo, командата ще върне грешка, защото обикновените потребители нямат право да инсталират софтуер!


3️⃣ apt-get – Какво е и за какво се използва?
📌 Какво е apt-get?
apt-get е пакетен мениджър за Linux (Ubuntu, Debian).
Използва се за инсталиране, обновяване и премахване на софтуер.
📌 Какво прави?
Изтегля и инсталира пакети от интернет.
Обновява вече инсталирани пакети.
Премахва стари пакети.
📌 Пример в кода:
yaml
    - name: Install Chrome
      run: |
         sudo apt-get update
         sudo apt-get install -y google-chrome-stable
📌 Какво означава това?

sudo apt-get update
Обновява списъка със софтуерни пакети.
Без него системата може да не намери най-новите версии.
sudo apt-get install -y google-chrome-stable
Инсталира Google Chrome.
-y означава "да" на всички въпроси, за да не пита потребителя.
    
    
📌 Таблица с обобщение
Команда	  Какво прави?	                   Защо се използва?	                                            Пример
env	      Създава променливи на средата	   Позволява програми да използват определени настройки	          CHROMEWEBDRIVER: /usr/bin/google-chrome
sudo	    Изпълнява команди с root права   Позволява на обикновен потребител да прави системни промени	  sudo apt-get install -y google-chrome
apt-get	  Инсталира/обновява софтуер	     Управлява пакети в Ubuntu/Debian	                              apt-get update и apt-get install




    English Version:

name: Selenium WebDriver CI  # The name of the CI/CD workflow that will be displayed in GitHub Actions.

on:  # Defines when this workflow should run.
  push:  # Runs when there is a push event.
    branches: [ "master" ]  # Only triggers when changes are pushed to the master branch.
  pull_request:  # Runs when a pull request is created.
    branches: [ "master" ]  # Only for the master branch.

jobs:  # Defines the jobs to be executed.
  build:  # Name of this job.

    runs - on: ubuntu - latest  # Runs on the latest available Ubuntu version.

    steps:  # List of steps that will be executed.
    -name: Checkout repository to the runner  # Downloads the repository to the runner.
      uses: actions / checkout@v4  # GitHub Action to clone the repository into the runner.

    - name: Setup.NET Core  # Installs the .NET SDK.
      uses: actions / setup - dotnet@v4  # GitHub Action for setting up the .NET environment.
      with:
        dotnet - version: 6.0.x  # Specifies that .NET 6.0 will be used.

    - name: Install Chrome  # Installs Google Chrome on the runner.
      run: |
         sudo apt - get update  # Updates the package list.
         sudo apt-get install -y google-chrome-stable  # Installs Chrome.

    - name: Install dependencies  # Installs the project dependencies.
      run: dotnet restore SeleniumBasicExercise.sln  # Restores all required packages for compilation.

    - name: Build  # Compiles the project.
      run: dotnet build SeleniumBasicExercise.sln --no-restore  
      # --no-restore means that `dotnet restore` will not run again.

    - name: Run TestProject1 tests  # Runs tests for TestProject1.
      env:  # Defines environment variables.
        CHROMEWEBDRIVER: / usr / bin / google - chrome  # Specifies the path to Chrome WebDriver.
      run: |
       echo "Running TestProject1 tests"  # Prints a message to the console.
       dotnet test TestProject1/TestProject1.csproj --no-build --verbosity normal  
       # --no-build means that tests will run without recompiling the project.

    - name: Run TestProject2 tests  # Runs tests for TestProject2.
      env: 
        CHROMEWEBDRIVER: / usr / bin / google - chrome
      run: |
       echo "Running TestProject2 tests"
       dotnet test TestProject2/TestProject2.csproj --no-build --verbosity normal  

    - name: Run TestProject3 tests  # Runs tests for TestProject3.
      env: 
        CHROMEWEBDRIVER: / usr / bin / google - chrome
      run: |
       echo "Running TestProject3 tests"
       dotnet test TestProject3/TestProject3.csproj --no-build --verbosity normal 
    
Explanation of Key Concepts
1️⃣ env – What is it and why is it used?
🔹 env stands for environment variables.
🔹 It is used to define settings or parameters that programs can access during execution.

📌 Example in the YAML file:

yaml

    - name: Run TestProject1 tests
      env: 
        CHROMEWEBDRIVER: / usr / bin / google - chrome
✅ What does this do?

The variable CHROMEWEBDRIVER is set to /usr/bin/google-chrome, which is the path where Chrome WebDriver is located.
When Selenium starts Chrome, it will use this WebDriver.
2️⃣ sudo – What is it and why is it used?
🔹 sudo stands for SuperUser DO.
🔹 It allows a regular user to execute administrative (root) commands.

📌 Example in the YAML file:

yaml

    - name: Install Chrome
      run: |
         sudo apt - get update
         sudo apt-get install -y google-chrome-stable
✅ What does this do?

sudo apt-get update updates the package list.
sudo apt-get install -y google-chrome-stable installs Google Chrome.
Without sudo, the installation would fail because normal users don't have permission to install system-wide software.


3️⃣ apt-get – What is it and why is it used?
🔹 apt-get is a package manager for Debian-based systems (like Ubuntu).
🔹 It is used to install, update, and remove software packages.

📌 Example in the YAML file:

yaml

    - name: Install Chrome
      run: |
         sudo apt - get update
         sudo apt-get install -y google-chrome-stable
✅ What does this do?

apt-get update updates the package list (without this, you might install outdated versions).
apt-get install -y google-chrome-stable installs Google Chrome.
The -y flag automatically confirms installation without prompting the user.


Summary Table
Command	    What does it do?	                 Why is it used?	                                    Example
env	        Sets environment variables	         Allows programs to use specific settings	            CHROMEWEBDRIVER: / usr / bin / google - chrome
sudo        Runs commands with root privileges	 Allows normal users to execute system-level commands	sudo apt-get install -y google-chrome
apt-get	    Installs/updates packages	         Manages software on Ubuntu/Debian	                    apt-get update && apt-get install -y google-chrome
