pipeline {
    parameters {
        string(name: 'Branch', defaultValue: 'master', description: 'Branch')
    }
    stages {
        stage('Build & Unit Test & Sonar') {
            steps {
                script {
                    projectCheckout("https://github.com/DIYvegAn/shipping-ac1.git")
                    // Encadena el build, los tests y sonar
                    sh "mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=AC1"
                }
            }
        }
        stage('Build Images') {
            steps {
                docker.withTool('docker') {
                    print "Instalado Docker"

                }
            }
        }
    }
}

def cleanImage(imageName, c) {
    try {
        c(imageName)
    } finally {
        ret = sh returnStatus: true, script: "docker rmi ${imageName}"
        if (ret != 0) {
            print("Ocurrio un error borrando la imagen ${imageName}")
        }
    }
}

def projectCheckout(String project) {
    checkout([$class                           : 'GitSCM',
              branches                         : [[name: "*/${params.Branch}"]],
              doGenerateSubmoduleConfigurations: false,
              // extensions                       : [[$class: 'RelativeTargetDirectory', relativeTargetDir: "${project}"]],
              gitTool                          : 'jgit',
              submoduleCfg                     : [],
              userRemoteConfigs                : [[credentialsId: '15940393-32ad-416e-ad80-b8ea71536641',
                                                   url          : "${project}"]]])
}