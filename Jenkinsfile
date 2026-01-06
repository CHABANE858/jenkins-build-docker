node {
    def app

    stage('Clone') {
        checkout scm
    }

    stage('Build image') {
        app = docker.build("xavki/nginx")
    }

    stage('Run image') {
        app.withRun('-p 8080:80') {
            sh 'docker ps'
            sh 'curl http://localhost:8080'
        }
    }
}
