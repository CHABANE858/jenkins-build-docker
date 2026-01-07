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
stage('Push') {
    docker.withRegistry('https://registry.gitlab.com', '6c5ab13e-5f0a-45b4-bd3e-8ee29b1c096e') {
        img.push('latest')          // tag latest
        img.push()                  // tag version-BUILD_ID
    }
}

