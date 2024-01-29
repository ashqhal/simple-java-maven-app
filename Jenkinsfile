pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            steps {
                sh '/opt/apache-maven-3.9.6/bin/mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh '/opt/apache-maven-3.9.6/bin/mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                script {
                    // Setting up Maven environment variables
                    env.MAVEN_HOME = '/opt/apache-maven-3.9.6'
                    env.M2_HOME = env.MAVEN_HOME
                    env.PATH = "${env.MAVEN_HOME}/bin:${env.PATH}"
                    
                    // Executing the deliver script
                    sh './jenkins/scripts/deliver.sh'
                }
            }
        }
        stage('Checkout') {
            steps {
                // Checkout code from GitHub repository
                git credentialsId: 'ashqhalani', url: 'https://github.com/ashqhal/simple-java-maven-app.git'
            }
        }
    }
}
