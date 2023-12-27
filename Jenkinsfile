pipeline {
    agent any
    tools {
        maven "maven3"
    }
    stages {
        stage('SCM') {
            steps {
                git 'https://github.com/dgp9999/course.git'
            }
        }
        stage('Build') {
            steps {
                sh "mvn clean package"
            }
        }
        stage('sonar') {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'rambo') {
                        sh "mvn sonar:sonar"
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sshagent(credentials: ['my_tom']) {
                        sh "scp -o StrictHostKeyChecking=no ${env.WAR_FILE_PATH} ubuntu@13.233.165.163:/home/ubuntu/tomcat9/webapps/myapp.war"
                        //sh "scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/Pipeline_Job/target/course.war ubuntu@13.233.165.163:/home/ubuntu/tomcat9/webapps/myapp.war"
                    }
                }
            }
        }
    }
}
