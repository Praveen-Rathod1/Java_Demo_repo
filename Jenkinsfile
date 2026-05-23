pipeline {
    agent any

    tools {
        maven 'maven3'
        jdk 'jdk17'
    }

    stages {

        stage('Maven Build') {
            steps {
                dir('java-demo-project') {

                    sh '''
                        mvn clean package -DskipTests

                        sudo rm -rf /opt/tomcat/webapps/tomcat-demo
                        sudo rm -rf /opt/tomcat/webapps/tomcat-demo.war

                        sudo cp target/tomcat-demo.war /opt/tomcat/webapps/
                    '''
                }
            }
        }

        stage('Deploy Tomcat') {
            steps {
                sh '''
                    sudo systemctl restart tomcat
                '''
            }
        }
    }
}
