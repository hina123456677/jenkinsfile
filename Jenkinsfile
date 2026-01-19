pipeline {
    agent any

    environment {
        BASE_GIT_URL = "https://github.com/hina123456677"
        GIT_CREDS    = "git-pat-creds"
    }

    stages {
        stage('Build Repositories') {
            steps {
                script {
                    // Load repos and branch defaults from config files
                    def repos = load 'reposConfig.groovy'
                    def branchConfig = load 'branchesConfig.groovy'

                    // Loop through all repositories dynamically
                    for (repo in repos) {
                        stage("Build ${repo.name}") {
                            dir(repo.name) {
                                // Pick branch: Jenkins param override or default from config
                                def branchToBuild = params[repo.param] ?: branchConfig[repo.param]
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
