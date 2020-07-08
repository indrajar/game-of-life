pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "myMaven"
    }
    parameters{
        string(name: 'userName', defaultValue: 'xy', description: 'parameter for the user to be ')
    }
    stages {
        stage("myParallelStage"){
            parallel{
             stage('yumUpdate'){
               steps{
                sh "sudo yum update -y"
                sh "sudo yum install git -y"
              }
            }    
                stage('createUser'){
                steps{
                sh "sudo useradd ${params.userName}"
                sh 'echo "${params.userName}  ALL=(ALL) NOPASSWD: ALL" | sudo tee -a /etc/sudoers'
            }
          }
        }
    }    
        
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/indrajar/game-of-life.git'

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'gameoflife-core/target/*.jar'
                }
            }
        }
    }
}

