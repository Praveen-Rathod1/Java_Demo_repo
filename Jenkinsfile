pipeline {
    agent any

    tools {
        maven 'maven3'
        jdk 'jdk17'
    }

    environment {
        WAR_NAME = 'tomcat-demo.war'
    }

    stages {

        stage('Maven Build') {
            steps {
                sh '''
                    mvn clean package -DskipTests

                    sudo rm -rf /opt/tomcat/webapps/tomcat-demo
                    sudo rm -rf /opt/tomcat/webapps/${WAR_NAME}

                    sudo cp target/${WAR_NAME} /opt/tomcat/webapps/
                '''
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh '''
                    sudo systemctl restart tomcat
                    sudo systemctl status tomcat --no-pager
                '''
            }
        }
    }
}
