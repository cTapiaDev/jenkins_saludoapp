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
                bat 'mvn clean compile'
            }
        }

        stage('Probar') {
            steps {
                bat 'mvn test'
            }
        }

        stage('SonarQube Anaylis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat 'mvn sonar:sonar'
                }
            }
        }

        stage('Análisis de Dependencias') {
            steps {
                bat 'mkdir dependency-check-report'
                tool 'OWASP_DC_CLI'
                dependencyCheck odcInstallation: 'OWASP_DC_CLI', additionalArguments: '--project "saludoapp" --scan "target" --format "HTML" --format "XML" --out "dependency-check-report" --enableExperimental'
            }
        }

        stage('Empaquetar') {
            steps {
                bat 'mvn package'
            }
        }

        
    }

    post {
        success {
            echo 'El build fue exitoso automáticamente'
        }

        failure {
            echo 'El build falló'
        }
    }
}