pipeline {
    agent any

    tools {
        maven 'Maven 3.9.10'
        jdk 'JDK21'
    }

    stages {
        stage('Clonar') {
            steps {
                // git 'https://github.com/cTapiaDev/jenkins_saludoapp'
                // git branch: 'main', url: 'https://github.com/cTapiaDev/jenkins_saludoapp'
                checkout scm
            }
        }

        stage('Compilar') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Probar') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Empaquetar') {
            steps {
                sh 'mvn package'
            }
        }
    }

    post {
        success {
            echo 'El build fue exitoso'
        }

        failure {
            echo 'El build falló'
        }
    }
}