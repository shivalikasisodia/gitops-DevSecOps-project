@Library('my-shared-library') _
pipeline {
    agent { label "Jenkins-Agent" }
    parameters{

        choice(name: 'action', choices: 'create\ndelete', description: 'Choose create/Destroy')
        string(name: 'ImageName', description: "name of the docker build", defaultValue: 'registration-app')
        string(name: 'GitUser', description: "GithubUsername", defaultValue: 'shivalikasisodia')
        string(name: 'ImageTag', description: "tag of the docker build", defaultValue: 'v1')
    }

    stages {
        stage('Git Checkout'){
        when { expression {  params.action == 'create' } }
            steps{
            gitCheckout(
                branch: "main",
                url: "https://github.com/shivalikasisodia/gitops-DevSecOps-project.git"
            )
            }
        }

        stage("Update the Deployment Tags") {
        when { expression {  params.action == 'create' } }
            steps {
                script{
                    updateDeploymentTag("${params.ImageName}","${params.ImageTag}")
                }
            }
        }

        stage("Push the changed deployment file to Git") {
        when { expression {  params.action == 'create' } }
            steps {
                pushDeploymentChange("${params.GitUser}")
            }
        }
      
    }
}
