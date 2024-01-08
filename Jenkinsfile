def containerName="insurence"
def tag="latest"
def docker_username="anjalisingh99"
def gitURL="https://github.com/Anjali-Singh-99/star-agile-insurance-project.git"

node {
    stage('checkout') {
        git changelog: false, credentialsId: 'GitHubCreds', poll: false, url: gitURL
    }
    stage('build') {
        sh "mvn clean install"
    }
    stage('clean any existing docker image') {
        sh "docker image prune -f"
    }
    stage('build the docker image') {
        sh "docker build -t $containerName:$tag --pull --no-cache ."
        echo "*****image build successfully completed******"
    }
    stage('push to docker hub') {
        withCredentials([usernamePassword(credentialsId: 'docker_user', passwordVariable: 'docker_pass', usernameVariable: 'docker_username')]) {
            sh "docker login -u $docker_username -p $docker_pass"
            sh "docker tag $containerName:$tag $docker_username/$containerName:$tag"
            sh "docker push $docker_username/$containerName:$tag"
            echo "***********image push successfully done*********"
            stash includes: '**/**.yml', name: 'deploy'
        }
    }
   
}
node("kubernetes_master"){
      stage('deploy the container'){
          unstash 'deploy'
          
                sh "sudo kubectl get nodes"
                sh "sudo kubectl apply -f deployment.yml"
                sh "sudo kubectl apply -f service.yml"
	         
	    }
	  
      }
