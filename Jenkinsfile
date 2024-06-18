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
        stage('SourceCode') {
            steps {
                git url: 'https://github.com/sunny2777/spring-petclinic.git',
                branch: 'sprint1_dev'
            }
        }
        stage('Build and static code analysis') {
            steps {
                wirthSonarQubeEnv('SONAT_LATEST'){
                    sh script: "mvn ${params.GOAL}"
                }
                
                stash name:'spc-build-jar', includes: 'target/*.jar'
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