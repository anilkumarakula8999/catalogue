pipeline {
    agent {
        docker {
            image 'node:18'
            args '-u root:root' // optional
        }
    }
    environment {
        COURSE = "jenkins"
        ACC_ID = "193685726527"
        PROJECT = "ROBOSHOP"
        COMPONENT = "catalogue"
        appversion = ""
    }
    stages {
        stage('Read version') {
            steps {
                script {
                    def packagejson = readJSON file: 'package.json'
                    env.appversion = packagejson.version
                    echo "App Version : ${env.appversion}"
                }
            }
        }
        stage('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Build Image') {
            steps {
                script {
                    withAWS(region: 'us-east-1', credentials: 'aws-creds') {
                        sh """
                            aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${env.ACC_ID}.dkr.ecr.us-east-1.amazonaws.com
                            docker build -t ${env.ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${env.PROJECT}/${env.COMPONENT}:${env.appversion} .
                            docker images
                            docker push ${env.ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${env.PROJECT}/${env.COMPONENT}:${env.appversion}
                        """
                    }
                }
            }
        }
    }
}
