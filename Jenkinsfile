pipeline {
    agent any

    tools {
        maven 'Maven'   // Name from Global Tool Configuration
        jdk 'Java17'    // Name of configured JDK
    }

    environment {
        ARTIFACTORY_SERVER = 'Artifactory' // Instance ID you set
        ARTIFACTORY_REPO = 'maven-local'   // Repository in Artifactory
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/jayanthis952/JFrog_artifactory_java_project.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Publish to Artifactory') {
            steps {
                script {
                    def server = Artifactory.server("${ARTIFACTORY_SERVER}")
                    def uploadSpec = """{
                        "files": [
                            {
                                "pattern": "target/*.jar",
                                "target": "${ARTIFACTORY_REPO}/"
                            }
                        ]
                    }"""
                    server.upload spec: uploadSpec
                    def buildInfo = Artifactory.newBuildInfo()
                    server.publishBuildInfo buildInfo
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
    }
}
