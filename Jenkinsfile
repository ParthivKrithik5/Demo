pipeline {
    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        ansiColor('xterm')
        timestamps() 
        timeout(time: 20, unit: 'MINUTES')
    }
    agent any
    parameters {
        string(name: 'DEPLOY_APP', default: 'force-web', description: 'Deploy this application or service')
        string(name: 'DEPLOY_VER', description: 'Deploy this version of the application or service')
        string(name: 'DEPLOY_ENV', default: 'dev', description: 'Deploy to this environment')
    }
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: '800ef5e8-589b-42ce-a235-0f08274f2921', url: 'https://github.com/ParthivKrithik5/Demo.git']]])
            }
        }
        stage('Setup') {
            steps {
                script {
                    sh """
                    pip install -r requirements.txt
                    """
                }
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
        stage('Deploy') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: "deploy-ssh-key", keyFileVariable: 'deploy.pem')]) {
                    sh """
                    ansible-playbook \
                    --key-file ./deploy.pem \
                    --extra-vars "environment=#{params.DEPLOY_ENV} version=#{params.DEPLOY_VER}" \
                    deploy_#{params.DEPLOY_APP}.yml
                    """
                }
            }
        }
        post {
            success {
                msg = "Deploy succeeded for #{params.DEPLOY_APP} #{params.DEPLOY_VER} " +
                "to #{params.DEPLOY_ENV} #{ (${env.BUILD_URL})"
                slackSend message: msg, channel: env.SLACK_CHANNEL
            }
            failure {
                script {
                    msg = "Deploy failed for #{params.DEPLOY_APP} #{params.DEPLOY_VER} " +
                    "to #{params.DEPLOY_ENV} #{ (${env.BUILD_URL})"
                    slackSend message: msg, channel: env.SLACK_CHANNEL
                }
            }
        }
        
    }
}
