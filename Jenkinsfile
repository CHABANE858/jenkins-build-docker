node {
    def app

    stage('Clone') {
        checkout scm
    }

    stage('Build image') {
        app = docker.build("xavki/nginx")
    }

    stage('Cleanup old containers') {
        sh 'docker rm -f $(docker ps -aq --filter ancestor=xavki/nginx) || true'
    }

    stage('Run image') {
        sh 'docker run -d -p 8093:80 xavki/nginx'
        sh 'docker ps'
        sh 'curl http://localhost:8093'
    }
}
