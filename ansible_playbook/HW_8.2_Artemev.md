Запустите ansible-lint site.yml и исправьте ошибки, если они есть.  
![Screenshot 2022-02-11 161010](https://user-images.githubusercontent.com/87374285/153741592-f83e250b-42e6-4733-8305-a04d148d3da1.png)  

Попробуйте запустить playbook на этом окружении с флагом --check.  
![Screenshot 2022-02-11 161207](https://user-images.githubusercontent.com/87374285/153741605-f311f29b-b0fd-4802-bd8a-c4ab0bb1f976.png)  

Запустите playbook на prod.yml окружении с флагом --diff. Убедитесь, что изменения на системе произведены.  
![Screenshot 2022-02-11 162513](https://user-images.githubusercontent.com/87374285/153741626-67452295-88ba-446b-8f64-e2e17ea44cd9.png)  

Повторно запустите playbook с флагом --diff и убедитесь, что playbook идемпотентен.  
![Screenshot 2022-02-11 163256](https://user-images.githubusercontent.com/87374285/153741634-c7607ba6-35ca-450d-92ae-2bcadf0044a7.png)  



