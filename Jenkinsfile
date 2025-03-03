pipeline {
    agent any

    triggers {
        cron('H/3 * * * 1') // Trigger the job every 3 minutes on Mondays
    }

    environment {
        // Define any environment variables here
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from GitHub
                git 'https://github.com/pateldev11/spring-petclinic'
            }
        }
        
        stage('Build') {
            steps {
                // Build the project using Maven
                script {
                    // Ensure Maven is installed and available
                    sh 'mvn clean install'
                }
            }
        }

        stage('Jacoco Coverage') {
            steps {
                // Generate the code coverage report using Jacoco
                script {
                    sh 'mvn jacoco:prepare-agent test jacoco:report'
                }
            }
            post {
                always {
                    // Archive the Jacoco coverage report
                    junit '**/target/test-*.xml'
                    jacoco(
                        execPattern: '**/target/jacoco.exec',
                        classPattern: '**/target/classes',
                        sourcePattern: '**/src/main/java'
                    )
                }
            }
        }

        stage('Package Artifact') {
            steps {
                // Generate the artifact (e.g., a JAR file) for deployment
                script {
                    sh 'mvn package'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
