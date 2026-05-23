pipeline {
    agent any

    tools {
        jdk 'jdk17'
        maven 'maven3'
    }

    stages {

        stage('Check Files') {
            steps {
                sh '''
                pwd
                ls -la
                find . -name pom.xml
                '''
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

    }
}
