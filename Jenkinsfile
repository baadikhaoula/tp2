
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

        stage('Publish Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKERHUB_USR',
                    passwordVariable: 'DOCKERHUB_PSW'
                )]) {
                    bat '''
                        echo %DOCKERHUB_PSW% | docker login -u "%DOCKERHUB_USR%" --password-stdin
                        if errorlevel 1 exit /b 1
                        docker push %IMAGE_NAME%
                        if errorlevel 1 exit /b 1
                    '''
                }
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
