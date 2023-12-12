pipeline {
    agent any
    options {
        // This is required if you want to clean before build
        skipDefaultCheckout(true)
    }
    stages {
        stage('remove') { 
            steps {
                script {
                    switch (BRANCH_NAME) {
                        case "development":
                            sh '''
                                docker compose -f mongo_and_redis_dockercomposer_development.yml up -d
                                docker rm -f mongo-qzkraft-ctr
                                docker rmi mongo:7.0
                                docker rm -f redis-qzkraft-ctr
                                docker rmi redis:7-alpine
                                '''
                            break
                        
                    }
                }
            }
        }
        stage('build') { 
            steps {
                // Clean before build
                cleanWs()
                // We need to explicitly checkout from SCM here
                checkout scm
                script {
                    switch (BRANCH_NAME) {
                        case "development":
                            sh '''
                            docker compose --env-file ../envconfig/mongo_redis.env -f mongo_and_redis_dockercomposer_development.yml up -d
                            '''
                            break
                        
                    }
                }
            }
        }
        stage('deploy') { 
            steps {
                script {
                    switch (BRANCH_NAME) {
                        case "development":
                            sh '''
                            echo 'redis host is ipaddres and user = default and password from env and port 6379'
                            echo 'mongo host is ipaddres and userand password you will get from env port 27017 '
                            '''
                            break
                        
                    }
                }
            }
        }
    }
    post {
        // Clean after build
        always {
            cleanWs(cleanWhenNotBuilt: false,
                    deleteDirs: true,
                    disableDeferredWipeout: true,
                    notFailBuild: true,
                    patterns: [[pattern: '.gitignore', type: 'INCLUDE'],
                               [pattern: '.propsfile', type: 'EXCLUDE']])
        }
    }
}