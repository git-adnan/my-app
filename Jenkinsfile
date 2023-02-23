 node{
   stage('SCM Checkout'){
     git 'https://github.com/git-adnan/my-app'
   }
   stage('maven-buildstage'){

      def mvnHome =  tool name: 'maven3', type: 'maven'  
      sh "${mvnHome}/bin/mvn clean package"
 sh 'mv target/myweb*.war target/newapp.war'
   }
   stage('SonarQube Analysis') {
       def mvnHome =  tool name: 'maven3', type: 'maven'
       withSonarQubeEnv('sonar') {
         sh "${mvnHome}/bin/mvn sonar:sonar"
       }
   }
   stage('Build Docker Imager'){
   sh 'docker build -t adnan1515/myweb:0.0.2 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u adnan1515 -p ${dockerPassword}"
    }
   sh 'docker push adnan1515/myweb:0.0.2'
   }
   stage('Nexus Image Push'){
   sh "docker login -u admin -p admin123 3.110.156.226:8083"
   sh "docker tag adnan1515/myweb:0.0.2 3.110.156.226:8083/adnan1515:1.0.0"
   sh 'docker push 3.110.156.226:8083/adnan1515:1.0.0'
   }
    stage('Remove Previous Container'){
    try{
    sh 'docker rm -f tomcattest'
    }catch(error){
    //  do nothing if there is an exception
    }
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest adnan1515/myweb:0.0.2'
   }
 }
}
