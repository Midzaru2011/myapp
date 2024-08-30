pipeline {
    environment {
        IMAGE_NAME = "midzaru2011/myapp"
        dockerImage = ""
        registryCredential = "dockerhub"
    }
    agent any

    stages {
        stage('Delete workspace') {
            steps {
                echo 'Deleting workspace'
                deleteDir()
            }
        }
        stage('Checkout branch'){
            steps{
                git branch:'main',
                url: 'https://github.com/Midzaru2011/myapp.git'
            }
        }
        stage ('Checkout tag'){
            steps {
                script {
                    sh 'git fetch'
                    IMAGE_TAG=sh(returnStdout:  true, script: "git tag --sort=-creatordate | head -n 1").trim()
                    echo "gitTag output: ${IMAGE_TAG}"
                }      
            }
        }
        stage('Build docker image') {
            steps {
                script {
                    dockerImage = docker.build "${IMAGE_NAME}"
                }
            }
        }
        stage('Push docker image:tag') {
            steps {
                script {
                 docker.withRegistry( 'https://index.docker.io/v1/', registryCredential ){
                    dockerImage.push("${IMAGE_TAG}")                    
                }
            }
        }
        }
        stage('Update tag'){
            steps{
                script{
                    sh "sed -i 's|tag:.*|tag: ${IMAGE_TAG}|g' ${env.WORKSPACE}/k8s/MyChart1/values.yaml"                    
                }
            }
        }
        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "Midzaru2011"
                   git add ${env.WORKSPACE}/k8s/MyChart1/values.yaml
                   git commit -m "Updated Deployment"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'GitHub', gitToolName: 'Default')]) {
                  sh "git push https://github.com/Midzaru2011/myapp.git main"
                }
            }
        }           
    }
}