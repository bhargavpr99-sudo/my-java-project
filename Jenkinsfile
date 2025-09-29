pipeline {
    agent any

    tools {
        maven 'Maven'      // Must match the name you set in Jenkins tool config
        jdk 'JDK17'        // Must match your configured JDK name
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
                        credentialsId: ''  // Leave blank if public repo
                    ]]
                ])
            }
        }

        stage('Build with Maven') {
            steps {
                echo "Building the project..."
                sh 'mvn clean package'
            }
        }

        stage('Run Application') {
            steps {
                echo "Running the Java application..."
                sh 'mvn exec:java'
            }
        }
    }

    post {
        success {
            echo "Build and execution successful!"
            // Archive your build artifacts here
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
        failure {
            echo "Build failed. Check logs for errors."
        }
    }
}
