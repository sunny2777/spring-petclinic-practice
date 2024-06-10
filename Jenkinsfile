node('Node-1'){
    stage('SourceCode'){
        git branch: 'sprint1_dev', url: 'https://github.com/sunny2777/spring-petclinic.git'
    }

    stage('Build'){
        sh 'mvn package'
    }

    stage('Archiving and Test Results'){
        junit stdioRetention: '', testResults: '**/surefire-reports/*.xml'
        archiveArtifacts artifacts: '**/*.jar', followSymlinks: false
    }
}