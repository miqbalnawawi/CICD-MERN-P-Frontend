env.DOCKER_REGISTRY = 'miqbalnawawi'
env.DOCKER_IMAGE_NAME = 'frontend3'
node('master') {
    stage('HelloWorld') {
        echo 'Hello World'
    }
    stage('Git Pull from Github') {
       git branch: 'main',url: 'https://github.com/miqbalnawawi/CICD-MERN-P-Frontend.git'
    }
    stage('Build Docker Image') {
        sh "docker build --build-arg APP_NAME=frontend -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER} ."   
    }
    stage('Push Docker Image to Dockerhub') {
        sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"
    }
    stage('DeployTo Kubernetes Cluster') {
        sh'''sed -i "15d" p-frontend-deployment.yml'''
        sh'''sed -i "14 a \'\\'          image: $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}" p-frontend-deployment.yml && sed -i "s/''//" p-frontend-deployment.yml'''
        sh 'kubectl apply -f p-frontend-deployment.yml'
   }
    stage('Remove Docker Image') {
        sh "docker rmi $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"   
    }
    stage("Clean Workspace"){
     cleanWs() 
        
    }
}   
