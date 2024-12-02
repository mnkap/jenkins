# 1. Введение

*Jenkins — это популярный инструмент для автоматизации процессов разработки, тестирования и доставки программного обеспечения. ВВ этом руководстве мы настроим Jenkins для выполнения задач CI/CD в вашем проекте.* 

# 2. Установка Jenkins
##  2.1. Системные требования

    Перед установкой убедитесь, что у вас установлен:

        * Java 8 или новее
        * Docker (по желанию, для контейнеризации Jenkins)
        * Операционная система: Linux (Ubuntu, CentOS), Windows или macOS

##  2.2. Установка на Ubuntu

    1. Добавьте ключ репозитория Jenkins:
        
   ```bash 
         wget -q -O - https://pkg.jenkins.io/jenkins.io.key | sudo apt-key add -
   ```

    2. Добавьте репозиторий Jenkins:

    ```bash
          sudo sh -c 'echo deb http://pkg.jenkins.io/debian/ stable main > /etc/apt/sources.list.d/jenkins.list'
    ```
    3. Обновите репозитории и установите Jenkins:

    ```bash
          sudo apt update
          sudo apt install jenkins
    ```
    4. Запустите Jenkins:

    ```bash
          sudo systemctl start jenkins
    ```
    5. Для запуска Jenkins автоматически при старте системы:

    ```bash
          sudo systemctl enable jenkins
    ```
##  2.3. Доступ к Jenkins

    После установки Jenkins доступен по умолчанию на порту 8080. Для доступа откройте браузер и перейдите по следующему адресу:
        (http://localhost:8080)
    
    При первом запуске вам будет предложено ввести пароль для разблокировки Jenkins. Этот пароль можно найти в файле:
    
    ```bash
          /var/lib/jenkins/secrets/initialAdminPassword
    ```
# 3. Конфигурация Jenkins
##  3.1. Установка необходимых плагинов

    После первого запуска Jenkins предложит установить плагины. Рекомендуется установить предложенные плагины или выбрать необходимые вручную.
    Некоторые полезные плагины для CI/CD:
       * Git
       * Pipeline
       * Docker Pipeline
       * Blue Ocean (для улучшенного UI)
       * JUnit (для отчетов о тестах)

## 3.2. Настройка интеграции с Git

    Перейдите в Manage Jenkins > Global Tool Configuration.
    В разделе Git настройте путь к исполнимому файлу Git, если он не найден автоматически.

## 3.3. Настройка секретов и учетных записей

    Для интеграции с внешними репозиториями, такими как GitHub или GitLab, настройте доступ к репозиториям через учетные данные:

        Перейдите в Manage Jenkins > Manage Credentials.
        Добавьте учетные данные для вашего репозитория (например, GitHub токен или SSH ключ).

#   4. Настройка Pipeline для CI/CD
##  4.1. Создание проекта Jenkins

    В Jenkins выберите New Item.
    Введите имя проекта и выберите Pipeline.
    Нажмите OK.