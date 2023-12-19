node {
    def app

    // Define DOCKERTAG as an environment variable
    environment {
        DOCKERTAG = "${env.BUILD_NUMBER}"
    }

    stage('Clone repository') {
        checkout scm
    }

    stage('Update GIT') {
        script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                    sh "git config user.email omkarr.patels@gmail.com"
                    sh "git config user.name Omkar Patel"
                    sh "cat deployment.yaml"
                    sh "sed -i 's+omkarpatel00/blue-test.*+omkarpatel00/blue-test:${DOCKERTAG}+g' dev/deployment.yaml"
                    sh "cat deployment.yaml"
                    sh "git add ."
                    sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                    sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/blue-kubernetesmanifest HEAD:main"
                }
            }
        }
    }
}
