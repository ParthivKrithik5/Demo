pipeline {
    agent any

    stages {
       
        stage('Compile Stage'){
            steps{
                withMaven(maven:'maven_3_8_3'){
                  sh 'mvn clean compile'
                }
            }
            
        }
        stage('Testing Stage'){
            steps{
                withMaven(maven:'maven_3_8_3'){
                  sh 'mvn test'
                }
            }
        }
    }
}
