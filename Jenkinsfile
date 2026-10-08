pipeline {
    agent any

    tools {
        maven 'Maven3'   // nom configuré dans Manage Jenkins > Tools
        jdk   'JDK17'    // idem
    }

    triggers {
        githubPush()     // déclenche le build à chaque push GitHub
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/GharbiMolka/DevOps-AppGestionDesProjets-main.git'
            }
        }

        stage('Tests unitaires') {
            steps {
                sh 'mvn clean test'          // sous Windows : bat 'mvn clean test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package -DskipTests' // génère le .jar dans target/
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success { echo 'Build réussi : livrable disponible dans target/' }
        failure { echo 'Échec du build' }
    }
}
