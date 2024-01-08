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
        }
    }
    stage('deploy the container') {
        node('kubernetes_master') {
            sh "kubectl apply -f deployment.yml"
            sh "kubectl apply -f service.yml"
        }
    }
}
