# Домашнее задание к занятию 9 «Процессы CI/CD»

## Подготовка к выполнению

1. Создайте два VM в Yandex Cloud с параметрами: 2CPU 4RAM Centos7 (остальное по минимальным требованиям).
2. Пропишите в [inventory](./infrastructure/inventory/cicd/hosts.yml) [playbook](./infrastructure/site.yml) созданные хосты.
3. Добавьте в [files](./infrastructure/files/) файл со своим публичным ключом (id_rsa.pub). Если ключ называется иначе — найдите таску в плейбуке, которая использует id_rsa.pub имя, и исправьте на своё.
4. Запустите playbook, ожидайте успешного завершения.
5. Проверьте готовность SonarQube через [браузер](http://localhost:9000).
6. Зайдите под admin\admin, поменяйте пароль на свой.
7.  Проверьте готовность Nexus через [бразуер](http://localhost:8081).
8. Подключитесь под admin\admin123, поменяйте пароль, сохраните анонимный доступ.


## Проведем подготовку к выполнению здания
1. запустим плайбук
```sh 
 ansible-playbook -i inventory/cicd/hosts.yml site.yml
```
![рис 1](https://github.com/ysatii/process-CI-CD/blob/main/img/img_1.jpg)  


2. Проверим работоспособность Nexus  
![рис 2](https://github.com/ysatii/process-CI-CD/blob/main/img/img_2.jpg)  

3. Проверим работоспособность SonarQube 
![рис 3](https://github.com/ysatii/process-CI-CD/blob/main/img/img_3.jpg)  


Часть репозиториев centos7 перестал использовать в 2024 году поэтому немного изменим скрипт 
добавим файфалы java и pgsql в папку files 
и ссылки на внешний репозиторий при желании данную работу моно помлностью повторить!



## Знакомоство с SonarQube

### Основная часть

1. Создайте новый проект, название произвольное.
2. Скачайте пакет sonar-scanner, который вам предлагает скачать SonarQube.
3. Сделайте так, чтобы binary был доступен через вызов в shell (или поменяйте переменную PATH, или любой другой, удобный вам способ).
4. Проверьте `sonar-scanner --version`.
5. Запустите анализатор против кода из директории [example](./example) с дополнительным ключом `-Dsonar.coverage.exclusions=fail.py`.
6. Посмотрите результат в интерфейсе.
7. Исправьте ошибки, которые он выявил, включая warnings.
8. Запустите анализатор повторно — проверьте, что QG пройдены успешно.
9. Сделайте скриншот успешного прохождения анализа, приложите к решению ДЗ.

## Знакомоство с SonarQube Выполнение задания
```sh
mkdir -p ~/tools && cd ~/tools
curl -O https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-7.0.2.4839-linux-x64.zip
sudo yum install -y unzip
unzip sonar-scanner-cli-7.0.2.4839-linux-x64.zip
echo 'export PATH=$HOME/tools/sonar-scanner-7.0.2.4839-linux-x64/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

```sh
sonar-scanner --version
```
ответ
```
22:39:08.711 INFO  Scanner configuration file: /home/lamer/tools/sonar-scanner-7.0.2.4839-linux-x64/conf/sonar-scanner.properties
22:39:08.714 INFO  Project root configuration file: NONE
22:39:08.731 INFO  SonarScanner CLI 7.0.2.4839
22:39:08.733 INFO  Java 17.0.13 Eclipse Adoptium (64-bit)
22:39:08.733 INFO  Linux 3.10.0-1160.119.1.el7.x86_64 amd64
```
Создадим проект  
![рис 4](https://github.com/ysatii/process-CI-CD/blob/main/img/img_4.jpg)  
![рис 5](https://github.com/ysatii/process-CI-CD/blob/main/img/img_5.jpg)  
![рис 6](https://github.com/ysatii/process-CI-CD/blob/main/img/img_6.jpg)  

Унас есть ошибки
![рис 7](https://github.com/ysatii/process-CI-CD/blob/main/img/img_7.jpg)  
![рис 8](https://github.com/ysatii/process-CI-CD/blob/main/img/img_8.jpg)  
![рис 9](https://github.com/ysatii/process-CI-CD/blob/main/img/img_9.jpg)  

ошибка более подробно, исправим ее и запустим свнова команду
```
sonar-scanner \
   -Dsonar.projectKey=test \
   -Dsonar.sources=. \
   -Dsonar.host.url=http://62.84.124.91:9000 \
   -Dsonar.login=a87256b19b5e039722df271df56af72154cfb359
```

![рис 10](https://github.com/ysatii/process-CI-CD/blob/main/img/img_10.jpg)  
![рис 11](https://github.com/ysatii/process-CI-CD/blob/main/img/img_11.jpg)  
![рис 12](https://github.com/ysatii/process-CI-CD/blob/main/img/img_12.jpg)  
![рис 13](https://github.com/ysatii/process-CI-CD/blob/main/img/img_13.jpg)  
![рис 14](https://github.com/ysatii/process-CI-CD/blob/main/img/img_14.jpg)  
![рис 15](https://github.com/ysatii/process-CI-CD/blob/main/img/img_15.jpg)  

Отчетыы указывают что ошибка исправлена!

## Знакомство с Nexus

### Основная часть

1. В репозиторий `maven-public` загрузите артефакт с GAV-параметрами:

 *    groupId: netology;
 *    artifactId: java;
 *    version: 8_282;
 *    classifier: distrib;
 *    type: tar.gz.
   
2. В него же загрузите такой же артефакт, но с version: 8_102.
3. Проверьте, что все файлы загрузились успешно.
4. В ответе пришлите файл `maven-metadata.xml` для этого артефекта.


http://51.250.92.164:8081/repository/maven-public/netology/java/maven-metadata.xml


```
<metadata modelVersion="1.1.0">
<groupId>netology</groupId>
<artifactId>java</artifactId>
<versioning>
<latest>8_282</latest>
<release>8_282</release>
<versions>
<version>8_102</version>
<version>8_282</version>
</versions>
<lastUpdated>20250403234932</lastUpdated>
</versioning>
</metadata>
```
Изменились ссылки на скачаивание файлов!
md5 sha1  одинкавы!


### Знакомство с Maven

### Подготовка к выполнению

1. Скачайте дистрибутив с [maven](https://maven.apache.org/download.cgi).
2. Разархивируйте, сделайте так, чтобы binary был доступен через вызов в shell (или поменяйте переменную PATH, или любой другой, удобный вам способ).
3. Удалите из `apache-maven-<version>/conf/settings.xml` упоминание о правиле, отвергающем HTTP- соединение — раздел mirrors —> id: my-repository-http-unblocker.
4. Проверьте `mvn --version`.
5. Заберите директорию [mvn](./mvn) с pom.

### Основная часть

1. Поменяйте в `pom.xml` блок с зависимостями под ваш артефакт из первого пункта задания для Nexus (java с версией 8_282).
2. Запустите команду `mvn package` в директории с `pom.xml`, ожидайте успешного окончания.
3. Проверьте директорию `~/.m2/repository/`, найдите ваш артефакт.
4. В ответе пришлите исправленный файл `pom.xml`.

---

### Как оформить решение задания

Выполненное домашнее задание пришлите в виде ссылки на .md-файл в вашем репозитории.

---
