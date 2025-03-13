pipeline {
    agent {
      label {
        label 'ubuntu-slave'
        retries 10
      }
    }

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }

    stages {
        stage('Build') {
            steps {
                // Clear Workspace
                cleanWs()
                
                // Get some code from a GitHub repository
                git branch: 'main', url: 'https://github.com/XadmaX/WildFly-Servlet-Example.git'

                // Run Maven on a Unix agent.
                sh "mvn clean package"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    archiveArtifacts artifacts: 'target/*.war', followSymlinks: false
                }
            }
        }
    }
}
