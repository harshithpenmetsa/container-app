stage('Push Docker Image') {
    steps {
        withCredentials([string(
            credentialsId: 'dockerhub-pat',
            variable: 'DOCKER_PAT'
        )]) {
            bat '''
            echo %DOCKER_PAT% | "%DOCKER_EXE%" login -u harshithpenmetsa --password-stdin
            "%DOCKER_EXE%" push %IMAGE_NAME%:%IMAGE_TAG%
            '''
        }
    }
}