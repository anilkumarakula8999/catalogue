// pre-build section
pipeline {
    agent any 
    environment {
        COURSE = "jenkins"
        appversion = ""
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
    
        stage('install dependencies') {
            steps {
                script {
                    sh """
                        npm install
                    """
                }
            
            }
        }

        stage('Deploy') {
            // input {
            //     message "Should we continue?"
            //     ok "Yes, we should."
            //     submitter "alice,bob"
            //     parameters {
            //         string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
            //     }
            // }
            // when {
            //     expression { "$params.DEPLOY" == true}
            // }
            steps {
                script {
                    sh """
                        echo "Deploying"
                        echo $COURSE
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