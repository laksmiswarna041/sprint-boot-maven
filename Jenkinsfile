pipeline {
    agent any

    stages {
        stage('build application'){
            steps{
            sh 'mvn clean package'
            }
        }

        stage('deploy image'){
            steps{
                sh 'docker build -t sgb-jenkins-task3 .'   
            }
        }
        stage('Tagging the build') {
            steps {
                sh 'git tag build-$BUILD_NUMBER'
                withCredentials([gitUsernamePassword(credentialsId: 'jenkins-task3', gitToolName: 'git-tool')]) {
                    sh "git push --tags"
                }
            }
        }
        stage('push to ECR'){
            steps{
                withDockerRegistry( [ credentialsId: "ecr:us-east-1:aws-creds", url: "https://590852515231.dkr.ecr.us-east-1.amazonaws.com" ] ){
                    sh 'docker tag sgb-jenkins-task3:latest 590852515231.dkr.ecr.us-east-1.amazonaws.com/sgb-jenkins-task3:$BUILD_NUMBER'
                    sh 'docker push 590852515231.dkr.ecr.us-east-1.amazonaws.com/sgb-jenkins-task3:$BUILD_NUMBER'
                }  

            }
        }
    }
}
