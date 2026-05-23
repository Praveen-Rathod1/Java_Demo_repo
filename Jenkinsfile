pipeline {
    agent any

    tools {
        maven 'maven3'
        jdk 'jdk17'
    }

    stages {

        stage('Maven Build') {
            steps {
                sh '''
                    pwd
                    find . -name pom.xml

                    mvn clean package -DskipTests

                    sudo rm -rf /opt/tomcat/webapps/tomcat-demo
                    sudo rm -rf /opt/tomcat/webapps/tomcat-demo.war

                    sudo cp target/tomcat-demo.war /opt/tomcat/webapps/
                '''
            }
        }

        stage('Deploy Tomcat') {
            steps {
                sh '''
                    sudo systemctl restart tomcat
                    sudo systemctl status tomcat --no-pager
                '''
            }
        }
    }
}
