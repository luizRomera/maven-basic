pipeline {
    agent {
        label 'slave'
    }
    
    stages {

        stage('Build') {
            steps {
                script {
                    sh 'mvn clean install'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    sh 'mvn test'
                }
            }
        }

        stage('Package') {
            steps {
                script {
                    sh 'mvn package'
                }
            }
        }

        stage('Publish Artifact') {
            steps {
                script {
                    archiveArtifacts "target/*.jar"
                }
            }
        }

        stage('Sonar') {
            steps {
                script {
                    sh """
                        mvn clean verify sonar:sonar -Dsonar.host.url=http://192.168.0.212:9000 -Dsonar.projectKey=spring-hello-world -Dsonar.login=sqp_aee8203c938509b280fd1716f3bde445cc018738
                    """
                }
            }
        }        

    }

    post {
        success {
            echo 'Build successful! Artifact generated and published.'
            jacoco(
                execPattern: '**/build/jacoco/*.exec',
                classPattern: '**/build/classes/java/main',
                sourcePattern: '**/src/main'
            )
        }
        failure {
            echo 'Build failed. Check the logs for more information.'
            echo 'Cleaning the environment.'
            sh 'rm -rf ~/.m2'
            sh 'rm -rf *'
        }
    }
}