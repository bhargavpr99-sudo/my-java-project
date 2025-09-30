pipeline {
    agent any

    tools {
        maven 'Maven'   // must match Maven name in Jenkins Global Tool Config
        jdk 'JDK17'     // must match JDK name in Jenkins Global Tool Config
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning GitHub project'
                checkout scm
            }
        }

        stage('Build WAR with Maven') {
            steps {
                echo 'Building WAR package...'
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                echo 'Deploying WAR to Tomcat...'
                sh '''
                    cp target/my-java-project-1.0-SNAPSHOT.war /opt/tomcat/webapps/ROOT.war
                    sudo systemctl restart tomcat
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Build and Deploy successful!'
        }
        failure {
            echo '❌ Build failed. Check logs for errors.'
        }
    }
}
