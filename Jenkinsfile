pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = "anasalwa/sonarpipelinedemo:latest"
        DOCKER_CREDENTIALS_ID = 'dockerhub_credentials'
        SONAR_HOST_URL = 'http://localhost:9000' // Variable d'environnement pour l'URL de SonarQube
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/AnaSalwa/SonarPipelineDemo.git', branch: 'main'
            }
        }

        stage('Compile') {
            steps {
                // Assurez-vous que Maven est installé et configuré dans Jenkins
                bat 'mvn clean compile'
            }
        }

        /*
        stage('SonarQube Analysis') {
            steps {
                script {
                    // Définir le chemin de SonarQube Scanner
                    def scannerHome = tool(name: 'sonar-scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation')

                    // Utilisation de withCredentials pour injecter le jeton SonarQube de manière sécurisée
                    withCredentials([string(credentialsId: 'sonarqubetoken', variable: 'SONAR_TOKEN')]) {
                        withSonarQubeEnv('SonaqubeServer') { // Remplacez par le nom de votre serveur SonarQube configuré dans Jenkins
                            bat "\"${scannerHome}\\bin\\sonar-scanner.bat\" " +
                                "-Dsonar.projectKey=TestPipeline " +
                                "-Dsonar.sources=. " +
                                "-Dsonar.host.url=${SONAR_HOST_URL} " +
                                "-Dsonar.login=${SONAR_TOKEN} " + // Utilisation du jeton sécurisé
                                "-Dsonar.java.binaries=./target/classes"
                        }
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        */

        stage('Build Docker Image') {
            steps {
                script {
                    // Ajout de logging pour suivre la construction de l'image Docker
                    echo "Starting Docker build..."
                    bat "docker build --no-cache -t ${DOCKER_IMAGE} ."
                    echo "Docker build completed"
                }
            }
        }
    }
}
