pipeline {
    agent any
    environment {
        JIRA_SITE = "https://rocklegs99.atlassian.net"  // 这里要和 Jenkins 配置里的 Jira Site 名称一致
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Frankilla99/RockLegs.git'
            }
        }
        stage('Update Jira - Start Build') {
            steps {
                script {
                    def issueKey = "TES-1"  // 这里替换成你的 Jira Issue Key
                    jiraAddComment idOrKey: issueKey, comment: "Build started: ${env.BUILD_URL}"
                    jiraTransitionIssue idOrKey: issueKey, transition: "In Progress"
                }
            }
        }
        stage('Build Application') {
            steps {
                echo "Building application..."
            }
        }
        stage('Deploy Application') {
            steps {
                echo "Deploying application..."
            }
        }
        stage('Update Jira - Build Complete') {
            steps {
                script {
                    def issueKey = "TES-1"
                    jiraAddComment idOrKey: issueKey, comment: "Build completed successfully!"
                    jiraTransitionIssue idOrKey: issueKey, transition: "Done"
                }
            }
        }
    }
}
