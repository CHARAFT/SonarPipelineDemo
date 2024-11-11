pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/aynuod/SonarPipelineDemo.git', branch: 'main'
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
                // script {
                //     def scannerHome = tool 'sonar-scanner'
                //     withSonarQubeEnv('SonarQube') {
                //         bat """
                //             ${scannerHome}\\bin\\sonar-scanner.bat ^
                //             -Dsonar.projectKey=team13_project ^
                //             -Dsonar.host.url=http://localhost:9000 ^
                //             -Dsonar.login=sqa_c9f7d5302e7679f2443c81297415e0b13f71a120 ^
                //             -Dsonar.sources=./src ^
                //             -Dsonar.java.binaries=./target/classes
                //         """
                //     }
                // }
                script {
                    // Définir le chemin de SonarQube Scanner
                    def scannerHome = tool name: 'sonarqube scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'

                   withSonarQubeEnv('sonar-server') { // Remplacez par le nom de votre serveur SonarQube
                    withCredentials([string(credentialsId: 'token1', variable: 'SONAR_TOKEN')]) {
                        bat "${scannerHome}\\bin\\sonar-scanner.bat \
                            -Dsonar.projectKey=team13_project \
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
