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
                    def scannerHome = tool 'sonar-scanner'
                    withSonarQubeEnv('SonarQube') {
                        bat """
                            ${scannerHome}\\bin\\sonar-scanner.bat ^
                            -Dsonar.projectKey=TestPipeline ^
                            -Dsonar.host.url=http://localhost:9000 ^
                            -Dsonar.login=sqa_112d17c5e3c02ec863d1ebc3717556c6745644d3 ^
                            -Dsonar.sources=./src ^
                            -Dsonar.java.binaries=./target/classes
                        """
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
