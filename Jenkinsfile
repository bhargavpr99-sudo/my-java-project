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
                        url: 'https://github.com/bhargavpr99-sudo/my-java-project.git',
                        credentialsId: ''
                    ]]
                ])
            }
        }

        stage('Build WAR with Maven') {
            steps {
                echo "Building WAR package..."
                sh 'mvn clean package'
            }
        }
    }

    post {
        success {
            echo "Build and WAR packaging successful!"
            archiveArtifacts artifacts: 'target/*.war', fingerprint: true
        }
        failure {
            echo "Build failed. Check logs for errors."
        }
    }
}
