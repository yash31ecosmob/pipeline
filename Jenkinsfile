#!groovy script
node {
def mailaddress = 'yash.d.rathod@gmail.com,manju.pothalappa@ecosmob.com'
//def webPath = '/home/docker/insinfinity'
//def dockerRegistry='dockerregistry.ecosmob.net:5000'

stage ('Checkout'){
checkout scm
}

//if (env.BRANCH_NAME == 'master')

 
stage('SonarQube analysis') {
                    // requires SonarQube Scanner 2.8+
                    //sh "npm install"
                    def scannerHome = tool 'Sonar-Scanner';
                    withSonarQubeEnv('SonarQube') {
                    def sample=env.JOB_NAME.replaceAll('/','.')
                    def projectKey=sample.replaceAll('%2F','.')
		    sh "${scannerHome}/bin/sonar-scanner -D sonar.host.url='http://3.108.64.24:9000/' -D sonar.projectKey=${projectKey}  -D sonar.sources=. -D sonar.exclusions=node_modules/**,Dockerfile,docker-compose.yml,default.conf"
                     stash includes: ".scannerwork/report-task.txt", name: 'sonar'
                    }
          }
	stage('send email notification') {
        emailext body: "Hello, \n Your Jenkins build for ${JOB_NAME} is ${currentBuild.currentResult}.\n You can verify your application by accessing URL set in environment file.", subject: "[Jenkins] Build status for ${JOB_NAME} ", to: "${mailaddress}"
	}
}
def Properties getProperties(filename) {
def properties = new Properties()
properties.load(new StringReader(readFile(filename)))
return properties
}
 
@NonCPS
def jsonParse(def json) {
new groovy.json.JsonSlurperClassic().parseText(json)
}
