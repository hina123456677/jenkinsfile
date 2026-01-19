pipeline {
    agent { label 'Jenkins' }

    environment {
        BASE_GIT_URL = "https://github.com/hina123456677"
        GIT_CREDS    = "git-pat-creds"
    }

    stages {
        stage('Build Repositories') {
            steps {
                script {
                    def repos = load 'reposConfig.groovy'

                    for (repo in repos) {
                        stage("Build ${repo.name}") {
                            dir(repo.name) {
                                // Branch from Jenkins param OR default in repo config
                                def branchToBuild = params[repo.param] ?: repo.branch
                                echo "📌 Building ${repo.name} on branch: ${branchToBuild}"

                                git branch: branchToBuild,
                                    url: "${env.BASE_GIT_URL}/${repo.name}.git",
                                    credentialsId: env.GIT_CREDS

                                sh repo.cmd
                            }
                        }
                    }
                }
            }
        }
    }

    post {
        success { echo "✅ All repositories built successfully" }
        failure { echo "❌ Build failed" }
    }
}
