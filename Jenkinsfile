pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/madhureddy121/springapp.git'
        WAR_FILE = 'target/springapp.war' // Change this if your WAR file is named differently
        REMOTE_USER = 'tomcatuser' // Replace with actual SSH user
        REMOTE_HOST = '203.0.113.10' // Replace with public IP of your Tomcat server
        REMOTE_TOMCAT_DIR = '/opt/tomcat/webapps/' // Replace with your actual webapps path
    }

    stages {
        stage('Checkout') {
            steps {
                git url: "${GIT_REPO}"
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Deploy WAR to Remote Tomcat') {
            steps {
                sshagent(credentials: ['ssh-key-id']) {
                    sh """
                        scp ${WAR_FILE} ${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_TOMCAT_DIR}
                    """
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment completed successfully.'
        }
        failure {
            echo '❌ Deployment failed.'
        }
    }
}
