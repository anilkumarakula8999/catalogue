// pre-build section
pipeline {
    agent any 
    environment {
        COURSE = "jenkins"
        appversion = ""
        ACC_ID = "193685726527"
        PROJECT = "ROBOSHOP"
        COMPONENT = "catalogue"
    }
    options {
        timeout(time: 10, unit: 'MINUTES')
        disableConcurrentBuilds()
    }
    // #build stages
    stages {
        stage('Read version') {
            steps { 
                script {
                    
                    def packagejson = readJSON file: 'package.json'
                    appversion = packagejson.version
                    echo "App Version : ${appversion}"
                   
                }

            }
        }

        stage('Install dependencies') {
            steps {
                script {
                    sh """
                        npm install
                    """
                }
            
            }
        }
        stage('Build Image') {
            steps {
                script {
                    withAWS(region:'us-east-1', credentials:'aws-creds') {
                        sh """
                            aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com
                            docker build ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appversion}
                            docker images
                            docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appversion}
                        """
                    }
                }
            }
        }

       
//   #post build 
    post{
        always{
              echo 'i will say always hello again'
              cleanWs() 
        }
        success{
              echo 'i will say success'
        }
        failure{
              echo 'i will say failure'
        }
        aborted{
              echo 'pipeline was aborted'
        }
    }
}
// end of pipeline line no 79