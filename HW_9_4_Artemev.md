###### 1. Сделать Freestyle Job, который будет запускать molecule test из любого вашего репозитория с ролью.  
![Screenshot 2022-03-20 190937](https://user-images.githubusercontent.com/87374285/159161420-c229a0ea-4116-43da-9c04-0ad6185507ee.png)  

###### 2. Сделать Declarative Pipeline Job, который будет запускать molecule test из любого вашего репозитория с ролью.  
![Screenshot 2022-03-20 220744](https://user-images.githubusercontent.com/87374285/159161559-883b0b0e-7987-46d3-aa88-2a4b7116a6f0.png)  

###### 3. Перенести Declarative Pipeline в репозиторий в файл Jenkinsfile.  
```
pipeline {
    agent {
        label 'ansible'
    }
    stages {
        stage('Checkout git') {
            steps{
              git branch: 'main', credentialsId: 'adf43e57-f21e-41a4-b472-ec5275436c83', url: 'git@github.com:iv-art074/elastic_template.git'
            }
        }
        stage('Install molecule') {
            steps{
                sh 'pip3 install -r test-requirements.txt'
                }
        }
        stage('Run Molecule'){
            steps{
                sh 'molecule test'
            }
        }
    }
}
```  

###### 4. Создать Multibranch Pipeline на запуск Jenkinsfile из репозитория.  
![Screenshot 2022-03-21 215103](https://user-images.githubusercontent.com/87374285/159255663-b355bf0f-816c-4510-baa3-d29c11403926.png)  

###### 5-7. Создать Scripted Pipeline, наполнить его скриптом из pipeline.  
Внести необходимые изменения, чтобы Pipeline запускал ansible-playbook без флагов --check --diff, если не установлен параметр при запуске джобы (prod_run = True), по умолчанию параметр имеет значение False и запускает прогон с флагами --check --diff.
Проверить работоспособность, исправить ошибки, исправленный Pipeline вложить в репозиторий в файл ScriptedJenkinsfile. Цель: получить собранный стек ELK в Ya.Cloud 

Файл создан, работоспособность проверена.
```
node("ansible"){  
    stage("Git checkout"){
            git branch: 'scripted-pipeline', credentialsId: '6f1ffff1-8a3f-4c07-ac74-5898288d4288', url: 'git@github.com:iv-art074/ansible_labs.git'
        }
    stage("Prod check"){
        secret_check=true
        prod_run = input(message: 'Is it prod job?', parameters: [booleanParam(defaultValue: false, name: 'prod_run')])
    }
    stage("Run playbook"){
             if (secret_check){
                dir('ansible_yandex/playbook') {
                if (prod_run){
                    sh 'ansible-playbook site.yml -i inventory/prod'
                    }
                else{
                    sh 'ansible-playbook site.yml -i inventory/prod --check --diff'
                    }
                }
            }
     }
}
```
###### 8. Отправить две ссылки на репозитории в ответе: с ролью и Declarative Pipeline и c плейбукой и Scripted Pipeline.
Pipeline
https://github.com/iv-art074/elastic_template/tree/main
Scripted
https://github.com/iv-art074/ansible_labs/tree/scripted-pipeline/ansible_yandex/playbook
