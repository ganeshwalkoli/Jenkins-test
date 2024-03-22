pipeline {
    agent any
    environment {
        BUILD_POD = sh(script: "kubectl get pods -o=jsonpath='{.items[0].metadata.name}' -n build-tool", returnStdout: true).trim()
        FRONTEND_POD = sh(script: "kubectl get pods -o=jsonpath='{.items[0].metadata.name}' -n starmf", returnStdout: true).trim()
        BACKEND_POD = sh(script: "kubectl get pods -o=jsonpath='{.items[0].metadata.name}' -n utility", returnStdout: true).trim()
        TAG = "${latestTag}"

    }

    stages {
        stage('Build') {
            steps {
                script {
                    // Checkout code including tags
                    git branch: 'master', url: 'https://github.com/ganeshwalkoli/angular_project.git', fetchTags: true

                     // Retrieve the latest tag again after potential abort
                    def latestTag = sh(script: 'git describe --tags $(git rev-list --tags --max-count=1)', returnStdout: true).trim()

                    // Retrieve the latest tag
                    def latestTag = sh(script: 'git describe --tags $(git rev-list --tags --max-count=1)', returnStdout: true).trim()
                    
                    echo "Latest tag is: $latestTag"
                    
                    //sh 'rm -rf *'
                    dir('angular-dir') {
                    checkout scm: [
                        $class: 'GitSCM',
                        userRemoteConfigs: [
                            [url: 'https://github.com/ganeshwalkoli/angular_project.git']
                        ],
                        branches: [
                            [name: "$latestTag"]
                        ]
                    ], poll: false
                    }
                    
                  }
            }
        }

    }
   post {
        success {
            echo 'Pipeline succeeded!!'
        }
        failure {
            echo 'Pipeline failed!'
        }
        unstable {
            echo 'Pipeline is unstable!'
        }
        aborted {
            echo 'Pipeline was aborted!'
        }
    }
}
