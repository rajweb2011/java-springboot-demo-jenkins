node {
    // reference to maven
    // ** NOTE: This 'maven-3.6.1' Maven tool must be configured in the Jenkins Global Configuration.   
    def mvnHome = tool 'MAVEN3'
    def dockerImage   
    def dockerRepoUrl = "localhost:8083"
    def dockerImageName = "hello-world-java"
    def dockerImageTag = "${dockerRepoUrl}/${dockerImageName}:${env.BUILD_NUMBER}"
    
    stage('Clone Repo') { 
      git 'https://github.com/rajweb2011/java-springboot-demo-jenkins.git'       
    }    
  
    stage('Build Project') {
      // build project via maven which is given above
      sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
    }
	
	stage('Publish Tests Results'){
      parallel(
        publishJunitTestsResultsToJenkins: {
          echo "Publish junit Tests Results"
		  junit '**/target/surefire-reports/TEST-*.xml'
		  archive 'target/*.jar'
        },
        publishJunitTestsResultsToSonar: {
          echo "This is branch "
      })
    }
		
    stage('Build Docker Image') {
      // build docker image hope you have installed docker on your local 
      sh "whoami"
      sh "ls -all /var/run/docker.sock"
      sh "mv ./target/hello*.jar ./data"   
      dockerImage = docker.build("hello-world-java")
    }
   
    stage('Deploy Docker Image'){
      
      // run local for testing 
      echo "Docker Image Tag Name: ${dockerImageTag}"
      sh "docker run -p 8081:8080 --rm hello-world-java"
    }
}
