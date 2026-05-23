pipeline {
    agent any

    tools {
        maven 'maven3'
        jdk 'jdk17'
    }

    stages {

        stage('Maven Build') {
            steps {

                dir('devopscode') {

                    sh '''
                        mvn clean package

                        sudo rm -rf /opt/tomcat/webapps/tomcat-demo
                        sudo rm -rf /opt/tomcat/webapps/tomcat-demo.war

                        sudo cp target/*.war /opt/tomcat/webapps/
                    '''
                }
            }
        }

        stage('Restart Tomcat Service') {
            steps {

                sh '''
                    sudo systemctl restart tomcat
                    sudo systemctl status tomcat --no-pager
                '''
            }
        }
    }
}
