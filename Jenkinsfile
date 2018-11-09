pipeline {
    agent any
    options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
    parameters {
        string(name: 'IMAGE_REPO_NAME', defaultValue: 'ummiyah/parse-server', description: '')
        string(name: 'DOCKER_SWARM_MANAGER', defaultValue: '192.168.3.224', description: 'Docker Swarm Manager Node IP Address')
        booleanParam(name: 'DOCKER_STACK_RM', defaultValue: false, description: 'Remove previous stack.  This is required if you have updated any secrets or configs as these cannot be updated. ')
    }
    stages {
        stage('git') {
            steps {
                git url: 'https://github.com/ummiyahnasim/parser-server.git'
            }
        }
        stage('Docker Build') {
            steps {
                sh '''#!/usr/bin/python
                      import docker
                      client = docker.DockerClient(base_url='tcp://192.168.3.224:2375')
                      build_image = client.images.build(path=${env.WORKSPACE},tag=ummiyah/parse-code:${env.BUILD_NUMBER},stream=True,encoding="gzip")'''
            }
        }
        stage('Docker Push') {
            steps {
                sh '''#!/usr/bin/python
                      import docker
                      client = docker.DockerClient(base_url='tcp://192.168.3.224:2375')
                      for line in client.images.push('ummiyah/parse-code:${env.BUILD_NUMBER}', stream=True):
                            print line
                      print "Docker Image successfully pushed to Docker Registry" '''
            }
        }
        stage(' Docker Deploy') {
            sh '''scp docker-compose.yml root@$DOCKER-SWARM-MANAGER:/home
                  ssh root@$DOCKER-SWARM-MANAGER "docker stack deploy -c /home/docker-compose.yml parse-server-stack" '''
        }
    }
}
