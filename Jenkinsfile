pipeline {
    agent any

    // C'est ici qu'on active l'écoute automatique
    triggers {
        githubPush() 
    }

    tools {
        maven "M3"
    }

    stages {
        stage('Build & Test') {
            steps {
                echo "🚀 Démarrage automatique..."
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
                        sh 'git config user.email "jenkins@bot.com"'
                        sh 'git config user.name "Jenkins CI"'
                        
                        // Astuce : On utilise le TIMESTAMP pour éviter les doublons si on rejoue le build
                        def tagName = "v1.0.${BUILD_NUMBER}"
                        
                        // On vérifie si le tag existe déjà pour ne pas faire planter le build
                        sh "git tag -a ${tagName} -m 'Auto-tag #${BUILD_NUMBER}' || echo 'Tag déjà existant'"
                        sh "git push https://${GIT_USER}:${GIT_PASS}@github.com/Mehdi031/HelloWorldMaven.git --tags || echo 'Push échoué ou déjà à jour'"
                    }
                }
            }
        }
    }
}
