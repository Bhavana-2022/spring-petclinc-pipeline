pipeline {
    agent { label 'JDK-17'}

    triggers {
        pollSCM('* * * * *')
    }

    tools {
        jdk 'JDK_17'
    }
    stages {
        stage('vcs') {
           steps { 
              git url: 'https://github.com/Praneethysp007/spcproject.git',
                  branch: 'develop'
           }
        }    
        stage('Build and package') {
           steps {
              sh script: 'mvn package'
           }
        }
        stage('artifacts and testresult') {
            steps {
               archiveArtifacts artifacts: '**/target/*.jar'
               junit testResults: '**/target/surefire-reports/*.xml'
            }
        }  
    }
    post {
        success {
            mail subject: 'your project is build succesful',
                 body : 'your spring project is succesful'
        }
        failure {
            mail subject: 'your project has failed',
                 body : 'your spring project is failed kindly check'
        }
    }
}