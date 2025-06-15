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
                tool 'OWASP_DC_CLI'
                dependencyCheck additionalArguments: '--project "saludoapp" --scan "target" --format "HTML" --format "XML"',
                                outputDirectory: 'dependency-check-report',
                                scanPath: 'target',
                                skipPublishedReports: false,
                                isCveRemoteEnabled: true,
                                isExperimentalEnabled: false,
                                failBuildOnCVSS: 7,
                                name: 'OWASP Dependency-Check Report'
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