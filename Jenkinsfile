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
                        sh 'docker login -u naveed0004 -p ${DOCKER_HUB}'
                    }
                    sh 'sudo docker push naveed0004/spring-integration'
                }
            }
        }
        stage('Deploy to k8s'){
            steps{
                script{
                    kubernetesDeploy configs: 'deployment.yaml', kubeConfig: [path: ''], kubeconfigId: 'kubernates-config-key', secretName: '', ssh: [sshCredentialsId: '*', sshServer: ''], textCredentials: [certificateAuthorityData: '', clientCertificateData: '', clientKeyData: '', serverUrl: 'https://']
                }
            }
        }
    }
}
