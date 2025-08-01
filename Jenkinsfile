pipeline {
    agent any
    environment {
        TOMCAT_URL = 'http://34.238.82.113:8090/'  // Replace with your Tomcat server IP
        TOMCAT_USER = 'admin'
        TOMCAT_PASS = 'admin'
        WAR_PATH = 'hello-world/webapp/target/webapp.war'
        CONTEXT_PATH = '/webapp'
    }
    stages {
        stage('cleanup') {
            steps {
                deleteDir() // deletes everything in the workspace
            }
        }
        stage('checkout') {
            steps {
                echo 'clonig remot repository....'
                sh 'git clone https://github.com/padalanaresh/hello-world.git'
            }
        }
        stage('build') {
            steps {
                echo 'Maven build initiated..'
                sh '''
                cd hello-world
                mvn clean package
                '''
            }
        }
        stage('deploy') {
            steps {
                echo 'Deploying WAR to Tomcat...'
                sh '''
                curl -v -u $TOMCAT_USER:$TOMCAT_PASS \
                --upload-file $WAR_PATH \
                "$TOMCAT_URL/manager/text/deploy?path=$CONTEXT_PATH&update=true"
                '''               
            }
        }
    }
}
