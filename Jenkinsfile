pipeline {
    agent any

    parameters {
        string(
            name: 'SERVER_IP',
            description: 'Tomcat Server IP'
        )
    }

    tools {
        jdk 'jdk17'
        maven 'maven3'
    }

    environment {
		SERVER_USER = 'ubuntu'
        TOMCAT_DIR = '/home/ubuntu/Maven_project/Java_Demo_repo/java-demo-project/src/main/webapp'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Praveen-Rathod1/Java_Demo_repo.git'
            }
        }

        stage('Build') {
            steps {

                dir('webapp') {
                    sh 'mvn clean package -DskipTests'
                }

                script {

                    env.WAR_FILE = sh(
                        script: "ls webapp/target/*.war",
                        returnStdout: true
                    ).trim()

                    env.WAR_NAME = sh(
                        script: "basename ${env.WAR_FILE}",
                        returnStdout: true
                    ).trim()

                    echo "WAR File Path: ${env.WAR_FILE}"
                    echo "WAR Name: ${env.WAR_NAME}"
                }
            }
        }


        stage('Deploy to Tomcat') {
            steps {

                sshagent(credentials: ['tomcat-ssh-key']) {

                    sh """
                        set -e

                        echo "Deploying WAR: ${WAR_NAME}"

                        scp -o StrictHostKeyChecking=no \
                        ${WAR_FILE} ${SERVER_USER}@${params.SERVER_IP}:/tmp/

                        ssh -o StrictHostKeyChecking=no \
                        ${SERVER_USER}@${params.SERVER_IP} '

                            sudo rm -rf ${TOMCAT_DIR}/webapp*

                            sudo mv /tmp/${WAR_NAME} ${TOMCAT_DIR}/

                            sudo systemctl restart tomcat

                            sleep 10

                            sudo systemctl status tomcat --no-pager
                        '

                        echo "Deployment completed successfully!"
                    """
                }
            }
        }
    }
}
