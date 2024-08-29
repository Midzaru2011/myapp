pipeline {
    environment {
        dockerImageName = "midzaru2011/myapp"
        dockerImage = ""
        registryCredential = "dockerhub"
    }
    agent any

    stages {
        stage('Delete workspace before build starts') {
            steps {
                echo 'Deleting workspace'
                deleteDir()
            }
        }
        stage('Checkout branch'){
            steps{
                git branch:'docker',
                url: 'https://github.com/Midzaru2011/myapp.git'
            }
        }
        stage ('Checkout tag'){
            steps {
                script {
                    sh 'git fetch'
                    gitTag=sh(returnStdout:  true, script: "git tag --sort=-creatordate | head -n 1").trim()
                    echo "gitTag output: ${gitTag}"
                }      
            }
        }
        stage('Build docker image') {
            steps {
                script {
                    dockerImage = docker.build dockerImageName
                }
            }
        }
        stage('Push docker image:tag'){
            steps{
                script {
                 docker.withRegistry( 'https://index.docker.io/v1/', registryCredential ){
                    dockerImage.push("${gitTag}")                    
                }
            }
        stage('sed env')
            environment{
                envTag = ("${gitTag}")
            }
            steps{
                script {
                    sh "sed -i \'s/gitTag/\'$envTag\'/g\'HelmCharts/MyChart1/values.yaml"
                }
            }
        }
    }
}
}