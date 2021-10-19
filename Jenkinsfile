#!groovy
pipeline {
    agent any
    triggers { pollSCM 'H/5 * * * *' }
    stages {
        stage('Source checkout') {
            steps {
                checkout(
                        [
                                $class                           : 'GitSCM',
                                branches                         : [],
                                browser                          : [],
                                doGenerateSubmoduleConfigurations: false,
                                extensions                       : [],
                                submoduleCfg                     : [],
                                userRemoteConfigs                : [
                                        [
                                                url: 'git@github.com:DIYvegAn/shipping-ac1.git'
                                        ]
                                ]
                        ]
                )
                stash 'dockerfile-source'
            }
        }
        stage('Build Images') {
            parallel {
                stage('DEV') {
                    agent any
                    environment {
                        AMBIENTE = 'DEV'
                    }
                    steps {
                        unstash 'dockerfile-source'
                        println "Ejecutando: ${env.AMBIENTE}"
                        script {
                            dockerImage = docker.build "${version}-${env.AMBIENTE}"
                        }
                    }
                }
                stage('STAGING') {
                    agent any
                    environment {
                        AMBIENTE = 'STAGING'
                    }
                    steps {
                        unstash 'dockerfile-source'
                        println "Ejecutando: ${env.AMBIENTE}"
                        script {
                            dockerImage = docker.build "${version}-${env.AMBIENTE}"
                        }
                    }
                }
                stage('PROD') {
                    agent any
                    environment {
                        AMBIENTE = 'PROD'
                    }
                    steps {
                        unstash 'dockerfile-source'
                        println "Ejecutando: ${env.AMBIENTE}"
                        script {
                            buildImage
                        }
                    }
                }
            }
        }
    }
}

void push(String registryCredential, String ambiente) {
    docker.withRegistry('', registryCredential) {
        dockerImage.push("${ambiente}")
        dockerImage.push('latest')
    }
}

String buildImage(String version, String imageName, List<String> Args) {
    def img = docker.build "${version}-${env.AMBIENTE}"
    return img
}