pipeline {
    agent any

    tools {
        maven "M3"
    }

    stages {
        stage('Build & Test') {
            steps {
                echo "🚀 Build depuis GitHub avec le nouveau script..."
                sh "mvn clean install"
            }
        }

        stage('Release & Tag') {
            when {
                expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'github-access-token', passwordVariable: 'GIT_PASS', usernameVariable: 'GIT_USER')]) {
                        
                        // Configurer l'identité Git
                        sh 'git config user.email "jenkins@bot.com"'
                        sh 'git config user.name "Jenkins CI"'

                        // Créer le tag
                        def tagName = "v1.0.${BUILD_NUMBER}"
                        sh "git tag -a ${tagName} -m 'Auto-tag from SCM #${BUILD_NUMBER}'"
                        
                        // Pousser le tag
                        sh "git push https://${GIT_USER}:${GIT_PASS}@github.com/Mehdi031/HelloWorldMaven.git --tags"
                    }
                }
            }
        }
    }
}
