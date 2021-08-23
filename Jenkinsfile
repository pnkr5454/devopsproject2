pipeline{
    agent any
    environment {
    def mvnHome = tool name: 'Maven3', type: 'maven'
    def mvnCMD = "${mvnHome}/bin/mvn"
    def DOCKER_TAG = "${getLatestCommitId()}"
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
    }
}
def dockerid(){
def commitid = sh returnStdout: true, script: 'git rev-parse --short HEAD'
return commitid
}
