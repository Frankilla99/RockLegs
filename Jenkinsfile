pipeline {
    agent any
    environment {
        JIRA_SITE = "https://rocklegs99.atlassian.net/"  // 在 Jenkins 配置的 Jira Server 名称
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/your-repo.git'
            }
        }

        stage('Extract Jira Issue') {
            steps {
                script {
                    // 读取最新 Git Commit Message
                    def commitMessage = sh(script: 'git log -1 --pretty=%B', returnStdout: true).trim()
                    
                    // 正则匹配 Jira Issue（如 PROJ-123）
                    def jiraIssuePattern = ~/([A-Z]+-[0-9]+)/   // 匹配 "ABC-123" 格式
                    def matcher = commitMessage =~ jiraIssuePattern
                    if (matcher.find()) {
                        env.JIRA_ISSUE = matcher.group(0)
                        echo "Detected Jira Issue: ${env.JIRA_ISSUE}"
                    } else {
                        echo "No Jira Issue found in commit message."
                        env.JIRA_ISSUE = ""
                    }
                }
            }
        }

        stage('Update Jira - Start Build') {
            when {
                expression { env.JIRA_ISSUE != "" }  // 仅当 Commit Message 里包含 Jira Issue 时执行
            }
            steps {
                script {
                    jiraAddComment idOrKey: env.JIRA_ISSUE, comment: "Build started: ${env.BUILD_URL}"
                    jiraTransitionIssue idOrKey: env.JIRA_ISSUE, transition: "In Progress"
                }
            }
        }

        stage('Build Application') {
            steps {
                sh 'echo "Building application..."'
                sh 'sleep 10'  // 模拟构建
            }
        }

        stage('Deploy Application') {
            steps {
                sh 'echo "Deploying application..."'
                sh 'sleep 5'  // 模拟部署
            }
        }
    }
    post {
        success {
            script {
                if (env.JIRA_ISSUE != "") {
                    jiraAddComment idOrKey: env.JIRA_ISSUE, comment: "Build successful: ${env.BUILD_URL}"
                    jiraTransitionIssue idOrKey: env.JIRA_ISSUE, transition: "Done"
                }
            }
        }
        failure {
            script {
                if (env.JIRA_ISSUE != "") {
                    jiraAddComment idOrKey: env.JIRA_ISSUE, comment: "Build failed! Check logs: ${env.BUILD_URL}"
                }
            }
        }
    }
}
