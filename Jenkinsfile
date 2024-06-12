pipeline {
    agent {label 'JDK17'}
    triggers { pollSCM('* * * * *') }
    parameters {
        choice(name: 'GOAL', choices:['compile', 'package', 'clean package'])
    }
    stages {
        stage('SourceCode') {
            steps {
                git branch: 'sprint1_dev', url: 'https://github.com/sunny2777/spring-petclinic.git'
            }
        }
        stage('Build') {
            steps {
                sh script: "mvn ${parms.GOAL}"
                stash name:'spc-build-jar', includes: 'target/*.jar'
            }
        }
        stage('Archiving and Test Results') {
            steps {
                junit stdioRetention: '', testResults: '**/surefire-reports/*.xml'
                archiveArtifacts artifacts: '**/*.jar', followSymlinks: false
            }
        }
        stage('deployment') {
            steps {
                unstash name: 'spc-build-jar'
                echo "clone the latest playbook"
                sh 'ansible --version'

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