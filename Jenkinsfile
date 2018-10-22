node {
 try {
  
  stage('Preparation') {
    git 'https://github.com/kurtkanaskie/apigee_CI_config_proxy_sharedflow.git'
    // mvnHome = tool 'M3'
    mvnHome = '/var/jenkins_home/tools/hudson.tasks.Maven_MavenInstallation/maven-3.3.9'
    sh "echo $WORKSPACE"
  }

try {
  stage('Deploy to Test') {
   // Run the maven build
   sh "'${mvnHome}/bin/mvn' -f proxies/pom.xml install -Ptest -Dorg=kurtkanaskietrainer-trial -Dapigee.username=YOUR_USERNAME -Dapigee.password=YOUR_PASSWORD -Dapigee.config.options=update validate"
   cucumber fileIncludePattern: '**/proxies/payment-v2/target/reports.json', sortingMethod: 'ALPHABETICAL'
   }
}catch (e) {
   //if tests fail, we can use a shell script which has 3 APIs to undeploy, delete current revision & deploy previous revision
   //sh "$WORKSPACE/undeploy.sh"
   throw e
  } finally {
   
  }
  
try {
  stage('Deploy to Production') {
   // Run the maven build
   sh "'${mvnHome}/bin/mvn' -f proxies/pom.xml install -Pprod -Dorg=kurtkanaskietrainer-trial -Dapigee.username='kurtkanaskie+jenkins@google.com' -Dapigee.password=Google2017 -Dapigee.config.options=update validate"
  }
  } catch (e) {
   //if tests fail, we can use a shell script which has 3 APIs to undeploy, delete current revision & deploy previous revision
   //sh "$WORKSPACE/undeploy.sh"
   throw e
  } finally {
   
  }
 } catch (e) {
  currentBuild.result = 'FAILURE'
  throw e
 } finally {
  
 }
}

