pipeline {
    agent any

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

        stage('SonarQube Analysis') {
            steps {

		script {
                    // Définir le chemin de SonarQube Scanner
                    def scannerHome = tool name: 'sonar-scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'

                   withSonarQubeEnv('SonaqubeServer') { // Remplacez par le nom de votre serveur SonarQube
                    withCredentials([string(credentialsId: 'sonarqubetoken', variable: 'SONAR_TOKEN')]) {
                        bat "${scannerHome}\\bin\\sonar-scanner.bat \
                            -Dsonar.projectKey=TestPipeline \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://localhost:9000 \
                            -Dsonar.login=%SONAR_TOKEN%"

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
    }
}
