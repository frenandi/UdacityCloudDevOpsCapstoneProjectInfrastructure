pipeline {
    environment {
        clusterCloudformationName = "UdacityCloudDevOpsClusterStack"
        clusterCloudformationFileName = "EKSClusterCloudFormation.yml"
        clusterCloudformationParameterFileName = "EKSClusterCloudFormationParameters.json"
        clusterNodeCloudformationName = "UdacityCloudDevOpsClusterNodeStack"
        clusterNodeCloudformationFileName = "EKSNodeCloudFormation.yml"
        clusterNodeCloudformationParameterFileName = "EKSNodeCloudFormationParameters.json"
    }
    agent any
    stages {
        stage('authorizing sh script run for jenkins'){
            steps{
                sh "chmod +x -R ${env.WORKSPACE}"
            }
        }
        stage('Deploy CloudFormation EKS Cluster') {
            steps{
                withAWS(region:'us-east-2',credentials:'awscredentials') {
                    sh "./create-stack.sh ${clusterCloudformationName} ${clusterCloudformationFileName} ${clusterCloudformationParameterFileName}"
                    sh "./validation-stack.sh ${clusterCloudformationName}"
                }
            }
        }
        stage('Deploy CloudFormation EKS ') {
            steps{
                withAWS(region:'us-east-2',credentials:'awscredentials') {
                    sh "./create-stack.sh ${clusterNodeCloudformationName} ${clusterNodeCloudformationFileName} ${clusterNodeCloudformationParameterFileName}"
                    sh "./validation-stack.sh ${clusterNodeCloudformationName}"
                }
            }
        }        
    }     
}