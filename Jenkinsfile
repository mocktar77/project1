
pipeline {
    agent any
    stages {
        stage("Deploy") {
            when {
                allOf {
                    branch "master"
                    environment(name: "ENV", value: "production")
                    tag "release-*"
                }
            }
            steps {
                echo "Deploy to production."
            }
        }
    }
}
