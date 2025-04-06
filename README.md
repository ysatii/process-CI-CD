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


https://github.com/ysatii/process-CI-CD/blob/main/maven-metadata.xml

Содержимое maven-metadata.xml
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
md5 sha1  одинаковы!

авторизировались
![рис 16](https://github.com/ysatii/process-CI-CD/blob/main/img/img_16.jpg)  

скопировали адрес репозитория
![рис 17](https://github.com/ysatii/process-CI-CD/blob/main/img/img_17.jpg) 

залили первый файл версия 8_102
![рис 18](https://github.com/ysatii/process-CI-CD/blob/main/img/img_18.jpg)  
![рис 19](https://github.com/ysatii/process-CI-CD/blob/main/img/img_19.jpg)  

Файлы в репозитории
![рис 20](https://github.com/ysatii/process-CI-CD/blob/main/img/img_20.jpg) 

контрольные суммы версии 8_102
![рис 21](https://github.com/ysatii/process-CI-CD/blob/main/img/img_21.jpg)  
![рис 22](https://github.com/ysatii/process-CI-CD/blob/main/img/img_22.jpg)  


![рис 23](https://github.com/ysatii/process-CI-CD/blob/main/img/img_23.jpg)  
![рис 24](https://github.com/ysatii/process-CI-CD/blob/main/img/img_24.jpg)  

файл версии 8_282
![рис 25](https://github.com/ysatii/process-CI-CD/blob/main/img/img_25.jpg)  

контрольные суммы версии 8_282
![рис 26](https://github.com/ysatii/process-CI-CD/blob/main/img/img_26.jpg)  
![рис 27](https://github.com/ysatii/process-CI-CD/blob/main/img/img_27.jpg)  

файл maven-metadata.xml
![рис 28](https://github.com/ysatii/process-CI-CD/blob/main/img/img_28.jpg)  
![рис 29](https://github.com/ysatii/process-CI-CD/blob/main/img/img_29.jpg)  
 

## Знакомство с Maven

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

## Выполнения задания Maven
### Подготовка к выполнению
1. Скачаваем дистрибутив [ссылка на дистрибутив](https://dlcdn.apache.org/maven/maven-3/3.9.9/binaries/apache-maven-3.9.9-bin.tar.gz)
```sh 
wget https://dlcdn.apache.org/maven/maven-3/3.9.9/binaries/apache-maven-3.9.9-bin.tar.gz
```

2. Разархивирование и установка переменной PATH 
```sh
mkdir -p ~/tools && tar -xzf apache-maven-3.9.9-bin.tar.gz -C ~/tools && echo 'export PATH=$HOME/tools/apache-maven-3.9.9/bin:$PATH' >> ~/.bashrc && source ~/.bashrc
```

3. Удалите из `apache-maven-<version>/conf/settings.xml` упоминание о правиле, отвергающем HTTP- соединение — раздел mirrors —> id: my-repository-http-unblocker. 
```sh
/home/lamer/apache-maven-3.9.9
sed -i '/<id>my-repository-http-unblocker<\/id>/,/<\/mirror>/d' conf/settings.xml
sed -i '/<mirror>/,/<\/mirror>/d' ~/tools/apache-maven-3.9.9/conf/settings.xml
```

4. Проверим `mvn --version`
```sh
mvn --version
```
ответ
```
Apache Maven 3.9.9 (8e8579a9e76f7d015ee5ec7bfcdc97d260186937)
Maven home: /home/lamer/apache-maven-3.9.9
Java version: 11, vendor: Oracle Corporation, runtime: /opt/jdk/openjdk-11+28_linux
Default locale: ru_RU, platform encoding: UTF-8
OS name: "linux", version: "3.10.0-1160.119.1.el7.x86_64", arch: "amd64", family: "unix"
```

5.````
```sh
mkdir mvn
nano pom.xml
```
### Основная часть
1. Поменяем в `pom.xml` блок с зависимостями под наш артефакт из первого пункта задания для Nexus (java с версией 8_282)
содержимое файла pom.xml
```
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.netology.app</groupId>
  <artifactId>simple-app</artifactId>
  <version>1.0-SNAPSHOT</version>

  <repositories>
    <repository>
      <id>my-repo</id>
      <name>maven-public</name>
      <url>http://89.169.138.205:8081/repository/maven-public/</url>
    </repository>
  </repositories>

  <dependencies>
    <dependency>
      <groupId>netology</groupId>
      <artifactId>java</artifactId>
      <version>8_282</version>
      <classifier>distrib</classifier>
      <type>tar.gz</type>
    </dependency>
  </dependencies>
</project>
```

![рис 30](https://github.com/ysatii/process-CI-CD/blob/main/img/img_30.jpg) 
![рис 31](https://github.com/ysatii/process-CI-CD/blob/main/img/img_31.jpg) 
![рис 32](https://github.com/ysatii/process-CI-CD/blob/main/img/img_32.jpg) 
![рис 33](https://github.com/ysatii/process-CI-CD/blob/main/img/img_33.jpg) 
![рис 34](https://github.com/ysatii/process-CI-CD/blob/main/img/img_34.jpg) 

sha1 353bcc3ee1d8cc15f42ff3420cd6c14cae902353
Скачивание произошло успешно!
исправленный      [mvn/pom.xml](https://github.com/ysatii/process-CI-CD/blob/main/mvn/pom.xml)


### Как оформить решение задания

Выполненное домашнее задание пришлите в виде ссылки на .md-файл в вашем репозитории.

---
