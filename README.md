# lesson6

**Домашнее задание**
1) Создать свой RPM пакет (можно взять свое приложение, либо собрать, например,
апач с определенными опциями)
2) Создать свой репозиторий и разместить там ранее собранную RPM

*Настраиваю VM для создания rpm пакета*
 
Требуемые зависимости для сборки пакета установливаюся с помощью vagrantfile	  
   
Скачиваю и разархивирую последний исходник для openssl (предложенныя ссылка в методичке не действительна)

      wget //https://www.openssl.org/source/old/1.1.1/openssl-1.1.1v.tar.gz
      tar -xvf latest.tar.gz

Ставлю зависимости для сборки

      yum-builddep rpmbuild/SPECS/nginx.spec

Собираю rpm пакет и проверяю

      rpmbuild -bb rpmbuild/SPECS/nginx.spec
      ll rpmbuild/RPMS/x86_64/

![image](https://github.com/movik242/lesson6/assets/143793993/8d08ac37-47eb-4d78-aece-6f7becf59a4f)


Устанавливаю локально и проверяю работу сервиса

      yum localinstall -y  /rpmbuild/RPMS/x86_64/nginx-1.20.2-1.el8.ngx.x86_64.rpm


![image](https://github.com/movik242/lesson6/assets/143793993/b6613c95-b1ae-4737-b0d9-68ee0c74bd6e)

![image](https://github.com/movik242/lesson6/assets/143793993/9c35cff3-0d69-4227-b899-d80e0ca79632)

Создаю свой репозиторий

      mkdir /usr/share/nginx/html/repo

      cp rpmbuild/RPMS/x86_64/nginx-1.20.2-1.el8.ngx.x86_64.rpm /usr/share/nginx/html/repo/

Ставлю Percona-Server

      wget https://downloads.percona.com/downloads/percona-distribution-mysql-ps/percona-distribution-mysql-ps-8.0.28/binary/redhat/8/x86_64/percona-
      orchestrator-3.2.6-2.el8.x86_64.rpm -O /usr/share/nginx/html/repo/percona-
      orchestrator-3.2.6-2.el8.x86_64.rpm

Инициализирую

      createrepo /usr/share/nginx/html/repo/

![image](https://github.com/movik242/lesson6/assets/143793993/37405112-9f86-4607-af99-964e2caae896)

В location / в файле /etc/nginx/conf.d/default.conf добавил директиву autoindex on. 

      location / {
      root /usr/share/nginx/html;
      index index.html index.htm;
      autoindex on;
      }

Добавил в /etc/yum.repos.d

      cat >> /etc/yum.repos.d/movik.repo << EOF
      [movik]
      name=movik-linux
      baseurl=http://localhost/repo
      gpgcheck=0
      enabled=1
      EOF


![image](https://github.com/movik242/lesson6/assets/143793993/73d43eae-c4f3-4300-a04b-93132cf31b3a)

Проверил доступность сайта

![image](https://github.com/movik242/lesson6/assets/143793993/46a196be-8e86-4d0c-a605-2464ace0e4ea)






