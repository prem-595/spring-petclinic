pipeline {
    agent { label 'JAVA' }
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
    }
}
