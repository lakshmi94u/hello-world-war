node{
    def mvnHome
    stage('SCM')
    {
        git 'https://github.com/lakshmi94u/hello-world-war.git'
        
        mvnHome= tool 'MVN_HOME'
    }
    stage('Build')
    {
        withEnv(["MVN_HOME=$mvnHome"])
        {
            if(isUnix())
            {
                sh '"$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore clean package'
            }else{
                bat(/"%MVN_HOME\bin\mvn" -Dmaven.test.failure.ignore clean package/)
            }
        }
    }
     stage('Deploy to Nexus') {
      // Run the maven build
      withEnv(["MVN_HOME=$mvnHome"]) {
         if (isUnix()) {
            sh '"$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore clean deploy'
         } else {
            bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean deploy/)
         }
      }
     }
    stage('Deploy')
    {
        deploy adapters: [tomcat9(credentialsId: '9c65371d-4cd4-4e3f-802d-fa0c1f8e9ac2', path: '', url: 'http://3.85.29.237:8090')], contextPath: 'hello-world', onFailure: false, war: '**/*.war'
    }
      stage('Sonar Analysis') {
      withEnv(["MVN_HOME=$mvnHome"]) {
         if (isUnix()) {
            sh '"$MVN_HOME/bin/mvn" sonar:sonar'
         } else {
            bat(/"%MVN_HOME%\bin\mvn" sonar:sonar/)
         }
      }
   }
}