node{
     
    stage('SCM Checkout'){
        git url: 'https://github.com/sandeepmaheshwaram/java-web-app-docker.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
    
    
    stage('Build Docker Image'){
        sh 'docker build -t dockersandy1/java-web-app .'
    }
    
    stage('Push Docker Image'){
       withCredentials([string(credentialsId: 'DOKCER_HUB_PASSWORD', variable: 'Docker_Hub_Pwd')]) {
          sh "docker login -u dockersandy1 -p ${Docker_Hub_Pwd}"
        }
        sh 'docker push dockersandy1/java-web-app'
     }
     
      stage('Run Docker Image In Dev Server'){
        
        def dockerRun = ' docker run  -d -p 8080:8080 --name java-web-app dockersandy1/java-web-app'
         
         sshagent(['docker']) {
          sh 'ssh -o StrictHostKeyChecking=no ubuntu@10.0.0.119 docker stop java-web-app || true'
          sh 'ssh  ubuntu@10.0.0.119 docker rm java-web-app || true'
          sh 'ssh  ubuntu@10.0.0.119 docker rmi -f  $(docker images -q) || true'
          sh "ssh  ubuntu@10.0.0.119 ${dockerRun}"
       }
       
    }
     
     
}
