# Домашнее задание к занятию "`GitLab`" - `Гуреев Юрий`

---

### Задание 1
Что нужно сделать:

Разверните GitLab локально, используя Vagrantfile и инструкцию, описанные в этом репозитории.
Создайте новый проект и пустой репозиторий в нём.

Зарегистрируйте gitlab-runner для этого проекта и запустите его в режиме Docker. Раннер можно регистрировать и запускать на той же виртуальной машине, на которой запущен GitLab.

В качестве ответа в репозиторий шаблона с решением добавьте скриншоты с настройками раннера в проекте.

## Решение
![Зарегистрированный runner](https://github.com/mhfhfmn/GitLab/blob/main/img/1.runner_seting_1.jpg)
![Настройка runner](https://github.com/mhfhfmn/GitLab/blob/main/img/2.runner_seting_2.jpg)

---

### Задание 2
Что нужно сделать:

Запушьте репозиторий на GitLab, изменив origin. Это изучалось на занятии по Git.
Создайте .gitlab-ci.yml, описав в нём все необходимые, на ваш взгляд, этапы.

В качестве ответа в шаблон с решением добавьте:

- файл gitlab-ci.yml для своего проекта или вставьте код в соответствующее поле в шаблоне;
- скриншоты с успешно собранными сборками.

## Решение

Файл .gitlab-ci.yml доступен по ссылке (ссылка на скриншот 2)
Содержимое файла:

```
stages:
  - test
  - build

variables:
  DOCKER_TLS_CERTDIR: ""
  DOCKER_HOST: "unix:///var/run/docker.sock"
  DOCKER_API_VERSION: "1.41"
#
test:
  stage: test
  image: golang:1.17
  script: 
   - go test .
  tags:
   - docker-runner

build:
  stage: build
  image: docker:latest
  script:
   - docker build -t myapp:latest .
   - docker save myapp:latest -o myapp-image.tar
  artifacts:
   name: "docker-image-$CI_COMMIT_SHORT_SHA"
   paths:
    - myapp-image.tar
  tags:
   - docker-runner
```

![Скриншот с выполненыйнным pipeline](https://github.com/mhfhfmn/GitLab/blob/main/img/3.ci_complet.jpg)
![Скриншот с получившимися artifacts](https://github.com/mhfhfmn/GitLab/blob/main/img/4.artifacts.jpg)
