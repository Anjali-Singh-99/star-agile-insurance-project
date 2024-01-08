def containerName="anjali-heath"
def tag="latest"
def dockerHubUser="anjalisingh99"
def gitURL="https://github.com/Anjali-Singh-99/star-agile-insurance-project.git"

node {
      stage('checkout') {
	         git changelog: false, credentialsId: 'GitHubCreds', poll: false, url: gitURL
	  }
	  stage('build') {
	         sh "mvn clean install"
	  }
	  stage('clean any existing docker image'){
	         sh "docker image prune -f"
	  }
	  stage('build the docker image'){
	         sh "docker build -t $containerName:$tag  --pull --no-cache ."
			 echo  "*****image build sucessfully completed******"
	  }
	  stage('push to docker hub'){
	      withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'docker_pass', usernameVariable: 'docker_user')]) {
                sh "docker login -u $docker_user -p $docker_pass"
				sh "docker tag $containerName:$tag $docker_user/$containerName:$tag"
				sh "docker push $docker_user/$containerName:$tag"
				echo "***********image push sucessfully done*********"
            }
	   
	  }
	  stage('deploy the container'){
	        
	  }
	  
     }
