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

        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'rambo') {
                        sh "mvn sonar:sonar"
                    }
                }
            }
        }

        stage('Nexus Upload') {
            steps {
                script {
                    def nexusArtifact = [
                        [artifactId: 'my-course', classifier: '', file: 'target/course.war', type: 'war']
                    ]

                    nexusArtifactUploader(
                        artifacts: nexusArtifact,
                        credentialsId: 'nexus_creds',
                        groupId: 'com.course',
                        nexusUrl: '13.232.20.107:8081',
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        repository: 'samplerepo',
                        version: '1.0.1'
                    )
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Use the actual path or environment variable based on your setup
                    def warFilePath = "${env.WORKSPACE}/target/course.war"

                    sshagent(credentials: ['my_tom']) {
                        sh "scp -o StrictHostKeyChecking=no ${warFilePath} ubuntu@13.233.165.163:/home/ubuntu/tomcat9/webapps/myapp.war"
                    }
                }
            }
        }

        stage('List All Stages') {
            steps {
                script {
                    def currentBuild = currentBuild.rawBuild
                    def executedStages = currentBuild.getAction(hudson.model.Cause.UserIdCause).getBuildCauses(currentBuild)
                            .collect { it.getShortDescription() }

                    echo "Executed Stages: ${executedStages.join(', ')}"
                }
            }
        }
    }
}
--------------------------------------------------------------------------
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

        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonag') {
                        sh "mvn sonar:sonar"
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
        /*

        stage('Nexus Upload') {
            steps {
                script {
                    def nexusArtifact = [
                        [artifactId: 'my-course', classifier: '', file: 'target/course.war', type: 'war']
                    ]

                    nexusArtifactUploader(
                        artifacts: nexusArtifact,
                        credentialsId: 'nexus_creds',
                        groupId: 'com.course',
                        nexusUrl: '13.232.20.107:8081',
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        repository: 'samplerepo',
                        version: '1.0.1'
                    )
                }
            }
        }
        */

        stage('Deploy') {
            steps {
                script {
                    // Use the actual path or environment variable based on your setup
                    //def warFilePath = "${env.WORKSPACE}/target/course.war"

                    sshagent(credentials: ['my_tom_boss2']) {
                        sleep 10
                        sh "scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/Demo_Pipe2/target/*.war ec2-user@13.232.138.178:/opt/tomct10/webapps/course.war"
                    }
                }
            }
        }
    }
}
