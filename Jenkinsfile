@Library('Shared')
import com.library.Shared

def shared = new Shared(this)
def repoName = "boxfuse-sample-java-war-hello"
 
pipeline {
    agent any
    
    stages {

        stage('github notification') {
            steps {
                echo "notifying github"
                script {
                    shared.setGithubStatus(repoName, "pending")
                }
            }
        }

        stage('unit tests') {
            steps {
                echo "running unit tests"
                script {
                    shared.mvn("test")
                }
                sh "sleep 30"
            }
        }

    }
    
    post {
        failure {
            script {
                shared.setGithubStatus(repoName, "failure")
            }
        }

        success {
            script {
                shared.setGithubStatus(repoName, "success")
            }
        }
    }

    options {
        disableConcurrentBuilds()
        buildDiscarder(logRotator(numToKeepStr: '3'))
    }

}