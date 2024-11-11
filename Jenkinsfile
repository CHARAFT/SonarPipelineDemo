pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/CHARAFT/SonarPipelineDemo.git', branch: 'main'
            }
        }

        stage('Compile') {
            steps {
                // Ensure Maven is installed and configured in Jenkins
                bat 'mvn clean compile'
            }
        }

        stage('SonarQube Analysis1') {
            steps {
                script {
                    // Define the path of SonarQube Scanner
                    def scannerHome = tool name: 'sonarqube scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'

                    withSonarQubeEnv('sonar-server') { // Replace with your SonarQube server name
                        withCredentials([string(credentialsId: 'token1', variable: 'SONAR_TOKEN')]) {
                            bat """
                                "${scannerHome}\\bin\\sonar-scanner.bat" ^
                                -Dsonar.projectKey=team13_project ^
                                -Dsonar.sources=./src ^
                                -Dsonar.java.binaries=./target/classes ^
                                -Dsonar.host.url=http://localhost:9000 ^
                                -Dsonar.login=%SONAR_TOKEN%
                            """
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
    }
}
