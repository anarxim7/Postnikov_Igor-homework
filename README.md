
### Задание 1

Что нужно сделать:

    Разверните GitLab локально, используя Vagrantfile и инструкцию, описанные в этом репозитории.
    Создайте новый проект и пустой репозиторий в нём.
    Зарегистрируйте gitlab-runner для этого проекта и запустите его в режиме Docker. Раннер можно регистрировать и запускать на той же виртуальной машине, на которой запущен GitLab.

В качестве ответа в репозиторий шаблона с решением добавьте скриншоты с настройками раннера в проекте.
![1.png](https://github.com/anarxim7/Postnikov_Igor-homework/blob/main/img/1.png)
![1.1png](https://github.com/anarxim7/Postnikov_Igor-homework/blob/main/img/1.1png)


---

### Задание 2

Что нужно сделать:

    Запушьте репозиторий на GitLab, изменив origin. Это изучалось на занятии по Git.
    Создайте .gitlab-ci.yml, описав в нём все необходимые, на ваш взгляд, этапы.

В качестве ответа в шаблон с решением добавьте:

    файл gitlab-ci.yml для своего проекта или вставьте код в соответствующее поле в шаблоне;

```
Поле для вставки кода...
....
....
stages:
  - build
  - test
  - deploy

variables:
    MAVEN_OPTS: "-Dmaven.repo.local=$CI_PROJECT_DIR/.m2/repository"

cache:
  paths:
    - .m2/repository/

build-job:
  stage: build
  script:
    - echo "Starting build process..."
    - echo "Compiling the code..."
    - mvn clean compile
    - echo "Build completed successfully!"
  artifacts:
    paths:
      - target/ 
    expire_in: 1 hour
  tags:
    - docker

test-job:
  stage: test
  script:
    - echo "Running tests..."
    - mvn test
    - echo "All tests passed!"
  dependencies:
    - build-job
  tags:
    - docker

deploy-job:
  stage: deploy
  script:
    - echo "Deploying application..."
    - docker build -t myapp:$CI_COMMIT_SHORT_SHA .
    - docker tag myapp:$CI_COMMIT_SHORT_SHA myapp:latest 
    - docker push $CI_REGISTRY_IMAGE:$CI_COMMIT_SHORT_SHA
    - docker stop myapp || true
    - docker rm myapp || true
    - docker run -d --name myapp -p 80:80 --restart=always myapp:latest
    - echo "Deployment completed!"
  only:
    - main
  dependencies:
    - build-job
  tags:
    - docker
....
....
```

![2.png](https://github.com/anarxim7/Postnikov_Igor-homework/blob/main/img/2.png)
![2.1png](https://github.com/anarxim7/Postnikov_Igor-homework/blob/main/img/2.1png)

