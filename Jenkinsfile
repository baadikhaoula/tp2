
pipeline {
    agent any

    environment {
        IMAGE_NAME = 'baadikhaoula/tp2:latest'
    }

    stages {
        stage('Cloning Git') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/baadikhaoula/tp2.git'
            }
        }

        stage('Building image') {
            steps {
                bat 'docker build -t %IMAGE_NAME% .'
            }
        }

        stage('Test image') {
            steps {
                bat 'docker image inspect %IMAGE_NAME%'
            }
        }

         stage('deploy image') {
            steps {
                bat '''
                    docker rm -f welcome-bdcc-pipeline 2>nul
                    docker run -d --name welcome-bdcc-pipeline -p 8084:80 %IMAGE_NAME%
                    if errorlevel 1 exit /b 1
                '''
            }
        }
    }
}
