pipeline {
    agent any
    environment {
        PROJECT_ID = 'red-delight-223804'
        CLUSTER_NAME = 'cision'
        LOCATION = 'us-central1-a'
        CREDENTIALS_ID = 'My-First-Project'
    }
    stages {
        stage("Checkout") {
            steps {
                checkout scm
            }
        }
        stage("Buildimage") {
            steps {
                script {
                    myapp = docker.build("impavithra/hello:${env.BUILD_ID}")
                }
            }
        }
        stage("Pushimage") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }
        stage("DeploytoGKE") {
            steps {
                sh "sed -i 's/hello:latest/hello:${env.BUILD_ID}/g' deployment.yml"
                //step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'deployment.yml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
                step([$class: 'KubernetesEngineBuilder', projectId: "red-delight-223804", clusterName: "cision", location: "us-central1-a", manifestPattern: 'deployment.yml', credentialsId: "My-First-Project", verifyDeployments: true])
            }
        }
    }
}
