node {
    def app

    stage('Clone') {
        checkout scm
    }

    stage('Build image') {
        // Construire l'image et stocker dans la variable app
        app = docker.build("registry.gitlab.com/chabane1997/jenkins-docker:version-${env.BUILD_ID}")
    }

    stage('Cleanup old containers') {
        sh 'docker rm -f $(docker ps -aq --filter ancestor=registry.gitlab.com/chabane1997/jenkins-docker:version-${BUILD_ID}) || true'
    }

    stage('Run image') {
        // Tester le conteneur
        app.withRun("-d -p 8999:80") {
            sh 'curl localhost:8094'
            sh 'docker ps'
        }
    }

    stage('Push') {
        // Pousser l'image sur GitLab
        docker.withRegistry('https://registry.gitlab.com', '6c5ab13e-5f0a-45b4-bd3e-8ee29b1c096e') {
            app.push('latest')          // tag latest
            app.push()                  // tag version-BUILD_ID
        }
    }
}
