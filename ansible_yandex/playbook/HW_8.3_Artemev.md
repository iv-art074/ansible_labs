Попробуйте запустить playbook на этом окружении с флагом --check  
![check3](https://user-images.githubusercontent.com/87374285/154792198-a8d9a2e9-8b33-4e7d-8f81-2079049d31fc.png)  

Запустите playbook на prod.yml окружении с флагом --diff. Убедитесь, что изменения на системе произведены.  
![diff3](https://user-images.githubusercontent.com/87374285/154792210-665a5687-3c4c-4d25-82d2-6cc5e0f50253.png)  

Проделайте шаги с 1 до 8 для создания ещё одного play, который устанавливает и настраивает filebeat.  
модули запущены:  
![Screenshot 2022-02-19 175155](https://user-images.githubusercontent.com/87374285/154792230-e131b0c6-ad40-495e-a029-7c2a9c935f7c.png)  

Playbook  

PLAY  
Устанавливаем Elastic  
загрузка установочного пакета  (get_url)  
установка в рабочий каталог из пакета (yum)  
создание по шаблону переменных окружений (templates)  

Устанавливаем Kibana  
загрузка установочного пакета  
установка в рабочий каталог из пакета  
создание по шаблону переменных окружений (templates)  

Устанавливаем Filebeat  
загрузка установочного пакета  
установка в рабочий каталог из пакета  
конфигурируем чере шаблон filebeat.yml.j2  
разрешаем модули - module enable  
производим первичную установку filebeat setup  
запускаем службы  service filebeat start  

Репо ссылка https://github.com/iv-art074/ansible_labs/tree/main/ansible_yandex/playbook 


