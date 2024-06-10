pipeline {
    agent {label 'Node-1'}
    triggers { pollSCM('* * * * *') }
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
}