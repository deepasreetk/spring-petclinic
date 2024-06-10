pipeline {
    agent {
        docker {
            image 'maven:3.8.5-jdk-11'
            args '-v /root/.m2:/root/.m2'
        }
    }

    environment {
        MAVEN_OPTS = '-Dmaven.repo.local=/root/.m2/repository'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/deepasreetk/spring-petclinic/', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('SonarQube Analysis') {
            environment {
                // Replace 'MySonarQubeServer' with your actual SonarQube server name in Jenkins
                SONARQUBE_SERVER = 'MySonarQubeServer'
            }
            steps {
                withSonarQubeEnv('MySonarQubeServer') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
