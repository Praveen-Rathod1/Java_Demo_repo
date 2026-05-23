pipeline {
    agent any

    tools {
        maven 'maven3'
        jdk 'jdk17'
    }
    
    environment {
		SERVER_USER = 'ubuntu'
        TOMCAT_DIR = '/opt/tomcat/webapps'
    }

    stages {

        stage('Maven Build') {
            steps {
                sh '''
                    mvn clean package -DskipTests

                    sudo rm -rf /opt/tomcat/webapps/tomcat-demo
                    sudo rm -rf /opt/tomcat/webapps/tomcat-demo.war

                    sudo cp target/*.war /opt/tomcat/webapps/
                '''
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

                            sudo rm -rf ${TOMCAT_DIR}/Java_demo_repo *

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
