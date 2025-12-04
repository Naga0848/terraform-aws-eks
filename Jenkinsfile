pipeline {
    agent {
        label 'AGENT-1'  // this is the Label which we mentioned while configuring the Jenkins agent and this pipeline gets exected, when these both the values match
    }
     environment { 
        appVersion = ''
        REGION = "us-east-1"
        ACC_ID = "843303282697"
        PROJECT = "roboshop"
     }
     options {
            timeout(time: 30, unit: 'MINUTES')  // this shows max time to run a pipeline
            disableConcurrentBuilds()   // this disables the parallel builds and used mostly in PROD pipelines
            ansiColor('xterm')   // to enable the colurs in the console output
             }
    stages {
        stage('init') {  // initiating terraform
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "Package version: ${appVersion}"
                }
            }
        }
        stage('Installing Dependencies') {  // this stage is to install the dependencies
            steps {
                sh """
                    npm install
                """
            }
        }
        stage('Docker Build') { // what ever we mentioned inside this stage are the commmands given in the AWS ECR itself, but we have to modify little according to our project
            steps {
                sh """
                    aws ecr get-login-password --region ${REGION} | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com
                    docker build -t ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion} .
                    docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion}
                    #aws ecr wait image-scan-complete --repository-name ${PROJECT}/${COMPONENT} --image-id imageTag=${appVersion} --region ${REGION}
                """
            }
        }
    }
}