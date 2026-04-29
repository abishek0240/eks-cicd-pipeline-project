pipeline {
 agent any

 environment {
 IMAGE_NAME="abishek1122/portfolio-cicd"
 IMAGE_TAG="${BUILD_NUMBER}"
 }

 stages {

 stage('Clone'){
 steps{
 git 'https://github.com/abishek0240/portfolio-cicd.git'
 }
 }

 stage('Build'){
 steps{
 sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
 }
 }

 stage('Push'){
 steps{
 withCredentials([
 usernamePassword(
 credentialsId:'dockerhub-creds',
 usernameVariable:'USER',
 passwordVariable:'PASS'
 )
 ]) {

 sh '''
 echo $PASS | docker login -u $USER --password-stdin
 docker push $IMAGE_NAME:$IMAGE_TAG
 '''

 }
 }
 }

 stage('Deploy'){
 steps{
 sh '''
 kubectl set image deployment/portfolio \
 portfolio=$IMAGE_NAME:$IMAGE_TAG
 '''
 }
 }

 }

}
