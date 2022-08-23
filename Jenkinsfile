pipeline{
    agent any
    tools{
         maven 'MAVEN_HOME'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/NaveedAmanat/SpringDemo.git']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'sudo docker build -t naveed0004/spring-integration .'
                }
            }
        }
        stage('Push Iamge to Docker Hub (naveed0004)'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'DOCKER_HUB_KEY', variable: 'DOCKER_HUB')]) {
                        
                    }
                }
            }
        }
        stage('Deploy to k8s'){
            steps{
                script{
                    withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'minikube', contextName: '', credentialsId: 'kube-token', namespace: 'default', serverUrl: 'https://10.0.2.15:8443']]) {
                        sh 'sudo kubectl delete svc springboot-k8s-svc'
                        sh 'sudo kubectl delete replicaSet spring-boot-k8s-rs'
                        sh 'sudo kubectl create -f deployment.yaml'
                    }
                }
            }
        }
    }
}
