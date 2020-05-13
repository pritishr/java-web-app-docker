node{

    def buildNumber = BUILD_NUMBER 
    // stage('SCM Checkout'){
    //     git url: 'https://github.com/pritishr00/java-web-app-docker.git',branch: 'master'
    // }
    
    stage(" Maven Clean Package"){
       def mavenHome =  tool name: "Maven", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
    
    
    stage('Build Docker Image'){
        sh "docker build -t pritishr00/java-web-app-docker:${buildNumber} ."
 
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'Docker_Hub_Pwd', variable: 'Docker_Hub_Pwd')]) {
          sh "docker login -u pritishr00 -p ${Docker_Hub_Pwd}"
        }
        sh "docker push pritishr00/java-web-app-docker:${buildNumber}"
     }
     
      stage('Run Docker Image In Dev Server'){
        
        //def dockerRun =  docker run  -d -p 8080:8080 --name javawebappcontainer dockerhandson/java-web-app-docker
         
         sshagent(['DOCKER_SERVER']) {
          sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.7.206 docker rm -f javawebappcontainer || true"
          //sh "ssh  ubuntu@172.31.7.206 docker rm javawebappcontainer || true"
          //sh 'ssh  ubuntu@172.31.20.72 docker rmi -f  $(docker images -q) || true'
          //sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.7.206 ${dockerRun}"
          sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.7.206 docker run  -d -p 8080:8080 --name javawebappcontainer pritishr00/java-web-app-docker${buildNumber}"
       }
     
    }
     
     
}
