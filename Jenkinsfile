pipeline {
    agent any
    environment{
        DOCKER_TAG = getDockerTag()
    }
    stages{
        stage('Build Docker image'){
            steps{
                sh "docker build . -t timbobb/nodeapp:${DOCKER_TAG}"
            }
        }
        stage('DockerHub Push'){
                steps{
                    withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerHubPwd')]) {
                    sh "docker login -u timbobb -p ${dockerHubPwd}"
                    sh "docker push timbobb/nodeapp:${DOCKER_TAG}"
                }
            }
        }
        stage('Deploy to Kubernetes'){
            steps{
                sh "chmod +x changeTag.sh"
                sh "./changeTag.sh ${DOCKER_TAG}"
            }
        }
    }
}

def getDockerTag(){
    def tag = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}
