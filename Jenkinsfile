pipeline {
    agent any
    triggers {
        pollSCM('*/1 * * * *')
    }
    stages {
        stage('build-docker-image') {
            steps {
                build()
            }
        }
         stage('deploy-dev') {
            steps {
                deploy("DEV")
            }
        }
        stage('test-dev') {
            steps {
                run_api_tests("DEV")
            }
        }
         stage('deploy-stg') {
            steps {
                deploy("STG")
            }
        }
        stage('test-stg') {
            steps {
                run_api_tests("STG")
            }
        }
         stage('deploy-prd') {
            steps {
                deploy("PRD")
            }
        }
        stage('test-prd') {
            steps {
                run_api_tests("PRD")
            }
        }
    }
}

def build(){
    echo "Building a docker image."
}

def deploy(String environment){
    echo "Deployment triggered on ${environment} env."
}

def run_api_tests(String environment){
    echo "API Tests triggered on ${environment} env."
}
