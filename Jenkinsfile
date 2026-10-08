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
        // Le checkout est fait automatiquement par "Pipeline script from SCM"

        stage('Tests unitaires') {
            steps {
                dir('backend') {
                    sh 'mvn clean test'
                }
            }
            post {
                always {
                    junit allowEmptyResults: true, testResults: 'backend/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package') {
            steps {
                dir('backend') {
                    sh 'mvn package -DskipTests'   // génère le .jar dans backend/target/
                }
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'backend/target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success { echo 'Build réussi : livrable disponible dans backend/target/' }
        failure { echo 'Échec du build' }
    }
}