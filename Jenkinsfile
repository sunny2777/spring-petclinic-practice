pipeline {
    agent {label 'JDK17'}
    options { 
        timeout(time: 1, unit: 'HOURS')
        retry(2) 
    }
    triggers { cron('0 * * * *') }
    parameters {
        choice(name: 'GOAL', choices:['compile', 'package', 'clean package'])
    }
    stages {
        stage('Source Code') {
            steps {
                git url: 'https://github.com/sunny2777/spring-petclinic.git', 
                branch: 'main'
            }
        }
         stage('Build the Code and sonarqube-analysis') {
            steps {
                withSonarQubeEnv('SONAR_LATEST') {
                    sh script: "mvn ${params.GOAL} sonar:sonar"
                }

            }
        }
        stage('Build') {
            steps {
                    sh script: "mvn ${params.GOAL}"
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