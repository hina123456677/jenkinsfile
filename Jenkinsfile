pipeline {
     agent { label 'Jenkins' }

    environment {
        BASE_GIT_URL = "https://github.com/hina123456677"
        GIT_CREDS   = "git-pat-creds"   // Jenkins credentials ID
        DEFAULT_BRANCH = "develop"
    }

    
 stage('Detect Branch') {
    stage('Detect Branch') {
        steps {
            script {
                def BUILD_BRANCH = env.BRANCH_NAME ?: DEFAULT_BRANCH
                echo "🔀 Building branch: ${BUILD_BRANCH}"
            }
    } 
}

    
    stages {
        stage('Build All Repositories') {
            steps {
                script {

                    def repos = [
                        [ name: 'ie-global',                cmd: "mvn -U -P '!tag' clean install" ],
                        [ name: 'ie-deps',                  cmd: "mvn -U -P '!tag' clean install" ],
                        [ name: 'ips-bo-api-specs',         cmd: "mvn -U -P '!tag' -DskipTests clean install" ],
                        [ name: 'ips-tch-backoffice',       cmd: "mvn -U -P '!tag,rpm,war' -DskipTests clean install" ],
                        [ name: 'ips-dsp',                  cmd: "mvn -U -P rpm -P '!tag' -DskipTests clean install" ],
                        [ name: 'ips-messages-canonical',   cmd: "mvn -U -P '!tag' -DskipTests clean install" ],
                        [ name: 'ips-messages-iso20022',    cmd: "mvn -U -P '!tag' -DskipTests clean install" ],
                        [ name: 'maps-service-config',      cmd: "mvn -U -P '!tag' -DskipTests clean install" ],
                        [ name: 'ips-maps-core',             cmd: "mvn -U -P '!tag,rpm' -DskipTests clean install" ],
                        [ name: 'ips-tch-database',         cmd: "mvn -U -P rpm,war -DskipTests clean install" ],
                        [ name: 'ips-tch-msimui',           cmd: "mvn -U -P '!tag,rpm' -DskipTests clean install" ],
                        [ name: 'ips-tch-switch',           cmd: "./build.sh" ],
                        [ name: 'ips-tch-transformer',      cmd: "mvn -U -P '!tag,rpm' -DskipTests clean install" ],
                        [ name: 'ips-transformer',          cmd: "mvn -U -P '!tag' -DskipTests clean install" ],
                        [ name: 'maps-tch',                 cmd: "mvn -U -P '!tag' -DskipTests clean install" ],
                        [ name: 'rambus-ste-client',        cmd: "mvn -U -P '!tag' -DskipTests clean install" ]

                        
                    ]

                    for (repo in repos) {
                        stage("Build ${repo.name}") {
                            dir(repo.name) {
                                git branch: DEFAULT_BRANCH,
                                    url: "${BASE_GIT_URL}/${repo.name}.git",
                                    credentialsId: GIT_CREDS

                                sh repo.cmd
                            }
                        }
                    }
                }
            }
        }
    }

    post {
        success {
            echo "✅ All repositories built successfully"
        }
        failure {
            echo "❌ Build failed"
        }
    }
}
}