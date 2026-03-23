pipeline {
    agent { label 'JAVA' }
     parameters{
        choice(name : 'mvn goals' , choices :['package','clean install','validate'],description:'pick something' )
    }
    stages {
        stage ('git checkout stage'){
            steps {
                git url: 'https://github.com/spring-projects/spring-petclinic.git', branch: 'main'
            }
        }
        stage ('build and sonarscan') {
            steps {
                    withCredentials([string(credentialsId:'sonar-token', variable: 'sonar')]) {
                    withSonarQubeEnv('SONAR') {
                        sh "mvn package sonar:sonar \
                            -Dsonar.projectKey=prem-595_spring-petclinic \
                            -Dsonar.organization=prem-595 \
                            -Dsonar.host.url=https://sonarcloud.io/ \
                            -Dsonar.login=${sonar}"
                    }
                }
            }
        }
      	stage("Quality Gate") {
	    steps {
	        timeout(time: 1, unit: 'HOURS') {
	        waitForQualityGate abortPipeline: true
	} 
    }
}
    }
}