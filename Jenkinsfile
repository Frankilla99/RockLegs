pipeline {
    agent { label "linux" }
    options {
        buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numKeepStr: '5')
        disableConcurrentBuilds()
    }
    stages {
        stage('Hello') {
            steps {
                echo "hello"
            }
        }
    }
}
