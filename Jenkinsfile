pipeline{
    agent any
    triggers{
       pollSCM('*/1 * * * *') 
    }
    environment {
        DOCKER_PASSWORD = credentials('docker-password')
        DOCKER_USERNAME = credentials('docker-username')
    }
    stages {
        stage('build-app'){
            steps {
                build("ataurins/python-greetings-app:latest", "Dockerfile")
            }
        }
        stage('deploy-dev'){
            steps {
                script {
                    deploy("DEV")
                }
            }
        }
        stage('test-dev'){
            steps {
                script {
                    test("DEV")
                }
            }
        }
        stage('approval'){
            steps {
                echo "Manual approval before deployment to PROD.."
            }
        }
        stage('deploy-prod'){
            steps {
                script {
                    deploy("PROD")
                }
            }
        }
        stage('test-prod'){
            steps {
                script {
                    test("PrOD")
                }
            }
        }
    }
    post {
        failure {
            script {
                echo "Pipeline failure.. Sending notification"
                // invoke dc plugin
            
            }
        }
        cleanup {
            echo "Cleanup procedure.."
            // potentionally some docker cleanup
        }
    }
}

def build(String tag, String dockerfile){
    echo "Building ${tag} image based on ${dockerfile}"
    sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
    sh "docker build --no-cache -t ${tag} . -f ${dockerfile}"
    sh "docker push ${tag}"
}

def test(String env) {
    echo "Testing of python-greetings-app on ${env} is starting.."
    // docker run .. 
    // docker exec
    // docker cp
    // extract report logic
    // docker cleanup
}

def deploy(String env) {
    echo "Deployment of python-greetings-app on ${env} is starting.."
    // kubectl set image ..
}