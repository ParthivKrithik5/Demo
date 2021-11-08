pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: '800ef5e8-589b-42ce-a235-0f08274f2921', url: 'https://github.com/ParthivKrithik5/Demo.git']]])
            }
        }
        stage('Build'){
            steps{
                git branch: 'main', credentialsId: '800ef5e8-589b-42ce-a235-0f08274f2921', url: 'https://github.com/ParthivKrithik5/Demo.git'
                bat 'python pipeline.py'
            }
            
        }
        stage('Compile Stage'){
            steps{
                withMaven(maven:'maven_3_8_3'){
                  sh 'mvn clean compile'
                }
            }
            
        }
        stage('Test'){
            steps{
                echo 'Testing done'
            }
        }
    }
}