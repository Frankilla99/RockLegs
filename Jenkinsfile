pipeline {
    agent any
    options {
        buildDiscarder logRotator(
            artifactDaysToKeepStr: '',
            artifactNumToKeepStr: '5',
            daysToKeepStr: '',
            numToKeepStr: '5'  // ✅ 这里改对了！
        )
        disableConcurrentBuilds()
    }
	stage('Update Jira') {
    steps {
        script {
            def issueKey = "Testing"
            jiraAddComment idOrKey: issueKey, comment: "Build started: ${env.BUILD_URL}"
            jiraTransitionIssue idOrKey: issueKey, transition: "In Progress"
        }
    }
}
}
