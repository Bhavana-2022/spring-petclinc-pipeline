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
              rtMavenDeployer (
                id: 'spc-daybuild',
                serverId: 'newinstance',
                releaseRepo: 'newereve-libs-release',
                snapshotRepo: 'newereve-libs-snapshot'
              )
              rtMavenRun (
                tool: 'DEFAULT',
                pom: 'pom.xml',
                goals: 'clean install',
                deployerId: 'spc-daybuild'
              )
              rtPublishBuildInfo (
                serverId: 'newinstance'

              )
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
                 body : 'your spring project is succesful',
                 to : 'praneeth@qt'
        }
        failure {
            mail subject: 'your project has failed',
                 body : 'your spring project is failed kindly check',
                 to : 'praneeth@qt'
        }
    }
}