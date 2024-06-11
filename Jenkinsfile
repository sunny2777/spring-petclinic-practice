pipeline {
    agent {label 'JDK17'}
    triggers { pollSCM('* * * * *') }
    Options {
        timeout(time: 1, unit: 'HOURS') 
    }
    stages {
        stage('SourceCode') {
            steps {
                git branch: 'sprint1_dev', url: 'https://github.com/sunny2777/spring-petclinic.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn package'
            }
        }
        stage('Archiving and Test Results') {
            steps {
                junit stdioRetention: '', testResults: '**/surefire-reports/*.xml'
                archiveArtifacts artifacts: '**/*.jar', followSymlinks: false
            }
        }
    }
     post {
        success {
            // send the success email
            echo "Success"
        }
        unsuccessful {
            //send the unsuccess email
            echo "failure"
        }
    }
}