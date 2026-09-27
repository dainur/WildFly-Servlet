pipeline {
    agent any
    
    tools {
        // Переконайтеся, що в Global Tool Configuration назва Maven збігається з "M3"
        maven "M3"
    }

    stages {
        stage('Build') {
            steps {
                // Очищення робочої директорії
                cleanWs()
                
                // Клонування репозиторію з кодом
                git branch: 'main', url: 'https://github.com/XadmaX/WildFly-Servlet-Example.git'

                // Збірка проекту Maven
                sh "mvn clean package"
            }

            post {
                success {
                    archiveArtifacts artifacts: 'target/*.war', followSymlinks: false
                }
            }
        }

        stage('Deploy to WildFly') {
            steps {
                script {
                    // 1. Шлях до згенерованого WAR-файлу
                    def warFile = 'target/devops-1.0-SNAPSHOT.war'
                    
                    // 2. Вкажіть точний шлях до директорії deployments вашого WildFly
                    def wildflyDeployDir = '/opt/wildfly/standalone/deployments/'

                    // 3. Копіювання артефакту
                    sh "cp ${warFile} ${wildflyDeployDir}"

                    // Якщо Jenkins працює від іншого користувача і виникає помилка доступу (Permission denied), 
                    // розкоментуйте рядок з sudo нижче:
                    // sh "sudo cp ${warFile} ${wildflyDeployDir}"
                }
            }
        }
    }
}
