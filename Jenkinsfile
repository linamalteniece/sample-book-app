pipeline {
    agent any
    triggers {
        pollSCM('*/1 * * * *')
    }
    stages {
        stage('build-docker-image') {
            steps {
                build_docker_image()
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

def build_docker_image(){
    echo "Building a docker image."
    sh "docker build -t lynnmal/sample-book-app ."

    echo "Pushing image to docker registry."
    sh "docker push lynnmal/sample-book-app"
}

def deploy(String environment){
    echo "Deployment triggered on ${environment} env."
    String lowercaseEnv  = environment.toLowerCase()
    sh "docker compose stop sample-book-app-${lowercaseEnv}"
    sh "docker compose rm sample-book-app-${lowercaseEnv}"
    sh "docker compose up -d sample-book-app-${lowercaseEnv}"
}

def run_api_tests(String environment){
    echo "API Tests triggered on ${environment} env."
    sh "docker pull lynnmal/js-api-tests"
    sh "docker run --network=host --rm lynnmal/js-api-tests run BOOKS BOOKS_${environment}"

}
