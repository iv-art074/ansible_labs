Запустите ansible-lint site.yml и исправьте ошибки, если они есть.  
![Screenshot 2022-02-11 161010](https://user-images.githubusercontent.com/87374285/153741592-f83e250b-42e6-4733-8305-a04d148d3da1.png)  

Попробуйте запустить playbook на этом окружении с флагом --check.  
![Screenshot 2022-02-11 161207](https://user-images.githubusercontent.com/87374285/153741605-f311f29b-b0fd-4802-bd8a-c4ab0bb1f976.png)  

Запустите playbook на prod.yml окружении с флагом --diff. Убедитесь, что изменения на системе произведены.  
![Screenshot 2022-02-11 162513](https://user-images.githubusercontent.com/87374285/153741626-67452295-88ba-446b-8f64-e2e17ea44cd9.png)  

Повторно запустите playbook с флагом --diff и убедитесь, что playbook идемпотентен.  
![Screenshot 2022-02-11 163256](https://user-images.githubusercontent.com/87374285/153741634-c7607ba6-35ca-450d-92ae-2bcadf0044a7.png)  

#### Playbook 
##### Переменные

GROUP VARS
java_oracle_jdk_package - имя пакета установки Java   
java_jdk_version - используемая версия Java (11.0.14)   
elastic_home - переменная домашнего каталога для Elasticsearch   
kibana_home - переменная для домашнего каталога для Kibana  
elastic_version - версия Elasticsearch (7.10.1)  
kibana_version - версия Kibana (8.0.0)   

##### PLAY  

###### Устанавливаем Java  
установлены тэги java для дальнейшего использования и отладки (tags)  
установка переменных (facts)  
загрузка установочного пакета  
создние рабочего каталога  
распаковка пакета (unarchive)  
создание по шаблону переменных окружений (templates)  

###### Устанавливаем Elastic
установлены тэги elastic для дальнейшего использования и отладки  
загрузка установочного пакета  
создание рабочего каталога  
распаковка в рабочий каталог из пакета  
создание по шаблону переменных окружений (templates)  

###### Устанавливаем Kibana  
установлены тэги kibana для дальнейшего использования и отладки  
загрузка установочного пакета
создание рабочего каталога
распаковка в рабочий каталог из пакета
создание по шаблону переменных окружений (templates)

