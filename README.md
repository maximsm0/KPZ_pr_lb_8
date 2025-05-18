
# Практично-лабораторне заняття №8
## Тема: Неперервна інтеграція
## Мета: ознайомитися з принципами і практиками неперервної інтеграції, сформувати навички автоматизації CI/CD процесів в GitHub Actions
# Завдання 1. GitHub Skills
1.1. Hello GitHub Actions
Репозиторій із виконаним завданням:
https://github.com/maximsm0/skills-hello-github-actions
1.2. Publish Packages
Репозиторій із виконаним завданням:
https://github.com/maximsm0/skills-publish-packages
Завдання 2. Власний GitHub Workflow
2.1. Створення workflow із двома тригерами
Для початку я запушив FE проект на github
![image](https://github.com/user-attachments/assets/966e7a6a-faa2-477b-a9fc-2182979733f5)
Створено файл .github/workflows/docker-build.yml, який запускається:
вручну через workflow_dispatch
автоматично при пуші в гілки main або feature/*
2.2. Опис workflow
Workflow складається з одного job, який виконує такі кроки:
Клонування репозиторію за допомогою actions/checkout@v4
Встановлення pnpm, інсталяція залежностей та збірка проєкту в одному скрипті:

    - name: Install pnpm and build project
      run: |
        npm install -g pnpm
        pnpm install
        pnpm run build
Авторизація в GitHub Container Registry через docker/login-action@v3:

    - name: Login to GitHub Container Registry
      uses: docker/login-action@v3
      with:
        registry: ghcr.io
        username: ${{ github.actor }}
        password: ${{ secrets.CR_PAT }}

Збірка та публікація Docker-образу через docker/build-push-action@v5:

    - name: Build and push Docker image
      uses: docker/build-push-action@v5
      with:
        context: .
        push: true
        tags: ghcr.io/${{ github.actor }}/${{ github.event.repository.name }}  
![image](https://github.com/user-attachments/assets/458d6de0-5794-4bdc-844d-2696c15ad601)
2.3. Перевірка
Після успішного виконання workflow Docker-образ з'явився у вкладці Packages в акаунті GitHub.
![image](https://github.com/user-attachments/assets/d875a58b-0525-42f4-aa94-7e9c705a0183)

# Висновок
Під час виконання лабораторної роботи я ознайомився з основами роботи GitHub Actions. Я навчився створювати власні workflow-файли з кількома тригерами запуску. Було реалізовано процес встановлення залежностей та збірки front-end застосунку за допомогою pnpm. Я також налаштував авторизацію та публікацію Docker-образу в GitHub Container Registry. У результаті виконання завдань, мій образ успішно з’явився у вкладці Packages мого акаунту.
