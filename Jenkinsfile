pipeline {
    agent any

    tools {
        maven 'Maven'      // Must match Jenkins tool config
        jdk 'JDK17'        // Must match Jenkins tool config
    }

    environment {
        MAVEN_OPTS = "-Dmaven.test.failure.ignore=true"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Cloning GitHub project"
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/master']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/YOUR-USERNAME/my-java-project.git',
                        credentialsId: '' // Add if repo is private
                    ]]
                ])
            }
        }

        stage('Build WAR with Maven') {
            steps {
                echo "Building the project..."
                sh 'mvn clean package'
            }
        }

        stage('Archive WAR') {
            steps {
                echo "Archiving WAR file..."
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            }
        }
    }

    post {
        success {
            echo "Build successful! WAR file ready for Tomcat deployment."
        }
        failure {
            echo "Build failed. Check logs for errors."
        }
    }
}
