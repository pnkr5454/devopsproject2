pipeline{
    agent any
    environment {
    def mvnHome = tool name: 'Maven3', type: 'maven'
    def mvnCMD = "${mvnHome}/bin/mvn"
    def DOCKER_TAG = "${dockerid()}"
    }
    stages{
        stage("checkout the code from github"){
            steps{
                git 'https://github.com/pnkr5454/devopsproject2'
            }
        }
        stage("build using maven"){
            steps{
                sh "${mvnCMD} clean package"
            }
        }
        stage("build the image using dockerfile"){
            steps{
                sh "docker build . -t pnkr5454/myapp01:${DOCKER_TAG}"
            }
        }
        stage("push the image to dockerhub"){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerpwd', usernameVariable: 'dockerusr')]) {
                 sh "docker login -u ${dockerusr} -p ${dockerpwd}"
                 sh "docker push pnkr5454/myapp01:${DOCKER_TAG} "    
                }
            }
        }
        stage("deploy using ansible"){
            steps{
                ansiblePlaybook credentialsId: 'deployservers', disableHostKeyChecking: true, 
                extras: "-e DOCKER_TAG=${DOCKER_TAG}", installation: 'ansible2', 
                inventory: 'dev.inv', playbook: 'ansiblebook.yml'
            }
        }
    }
}
def dockerid(){
def commitid = sh returnStdout: true, script: 'git rev-parse --short HEAD'
return commitid
}
