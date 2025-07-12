pipeline{
    agent {
        label "Slave"
    }
    environment {
        DOCKER_REGISTRY = "quay.okd.ir:8443/ubi8"
        IMAGE_NAME = "wmm6"
        deployment_name = "wmm6"
        PROJECT_ROOT = "."
    }
    stages {
        stage("build"){
            steps{
                updateGitlabCommitStatus(name: "build", state: "running")
                dir("${PROJECT_ROOT}") {
				
                    sh "docker compose build --no-cache"
                    sh "docker compose push"
                }
            }
            post {
                success {
                    updateGitlabCommitStatus(name: "build", state: "success")
                }
                unsuccessful {
                    updateGitlabCommitStatus(name: "build", state: "failed")
                }
                aborted {
                    updateGitlabCommitStatus(name: "build", state: "canceled")
                }
                always {
                    sh "docker rmi -f \$(docker images --filter 'dangling=true' -q --no-trunc) | true"
                }
            }
        }
    }
}
