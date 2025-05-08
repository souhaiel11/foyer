pipeline {
    agent any

    environment {
        SONAR_PROJECT_KEY = 'projet-dev '  // 🔁 Remplace par le nom réel de ton projet Sonar
        SONAR_HOST_URL = 'http://localhost:9000' // 🔁 ou l’URL de ton serveur Sonar
        SONAR_LOGIN = credentials('sonar-token') // 🔁 ID du token stocké dans Jenkins
    }

    stages {
        stage('GIT') {
            steps {
                echo "Getting Project from Git"
                git branch: 'main', credentialsId: 'github-token', url: 'https://github.com/souhaiel11/foyer.git'
            }
        }

        stage('MVN CLEAN') {
            steps {
                sh 'mvn clean'
            }
        }

        stage('MVN COMPILE') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo " Running SonarQube analysis"
                withSonarQubeEnv('MySonarServer') {
                    sh """
                        mvn sonar:sonar \
                        -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                        -Dsonar.host.url=${SONAR_HOST_URL} \
                        -Dsonar.login=${SONAR_LOGIN}
                    """
                }
            }
        }
    }
}
