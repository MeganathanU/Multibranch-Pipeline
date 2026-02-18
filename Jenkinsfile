pipeline {
    agent any
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
                script {
                    echo "Building branch: ${env.BRANCH_NAME}"
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh """
                        ${SCANNER_HOME}/bin/sonar-scanner \
                        -Dsonar.projectKey=python-app \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=$SONAR_HOST_URL
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    if (env.BRANCH_NAME == "master") {
                        sh """
<<<<<<< HEAD
                            docker build -t vijay3639/masterimage . 
                            docker tag vijay3639/masterimage:latest vijay3639/masterimage:${BUILD_NUMBER}
                        """
                    } else if (env.BRANCH_NAME == "developer") {
                        sh """
                            docker build -t vijay3639/devimage . 
                            docker tag vijay3639/devimage:latest vijay3639/devimage:${BUILD_NUMBER}
=======
                            docker build -t meganathanu/masterimage . 
                            docker tag meganathanu/masterimage:latest meganathanu/masterimage:${BUILD_NUMBER}
                        """
                    } else if (env.BRANCH_NAME == "developer") {
                        sh """
                            docker build -t meganathanu/devimage . 
                            docker tag meganathanu/devimage:latest meganathanu/devimage:${BUILD_NUMBER}
>>>>>>> 5ab40e5 (test)
                        """
                    }
                }
            }
        }

        stage('Trivy Scan') {
            steps {
                script {
                    if (env.BRANCH_NAME == "master") {
<<<<<<< HEAD
                        sh "trivy image --exit-code 0 --severity HIGH,CRITICAL vijay3639/masterimage:latest"
                    } else if (env.BRANCH_NAME == "developer") {
                        sh "trivy image --exit-code 0 --severity HIGH,CRITICAL vijay3639/devimage:latest"
=======
                        sh "trivy image --exit-code 0 --severity HIGH,CRITICAL meganathanu/masterimage:latest"
                    } else if (env.BRANCH_NAME == "developer") {
                        sh "trivy image --exit-code 0 --severity HIGH,CRITICAL meganathanu/devimage:latest"
>>>>>>> 5ab40e5 (test)
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'

                    script {
                        if (env.BRANCH_NAME == "master") {
                            sh """
<<<<<<< HEAD
                                docker push vijay3639/masterimage:latest
                                docker push vijay3639/masterimage:${BUILD_NUMBER}
                            """
                        } else if (env.BRANCH_NAME == "developer") {
                            sh """
                                docker push vijay3639/devimage:latest
                                docker push vijay3639/devimage:${BUILD_NUMBER}
=======
                                docker push meganathanu/masterimage:latest
                                docker push meganathanu/masterimage:${BUILD_NUMBER}
                            """
                        } else if (env.BRANCH_NAME == "developer") {
                            sh """
                                docker push meganathanu/devimage:latest
                                docker push meganathanu/devimage:${BUILD_NUMBER}
>>>>>>> 5ab40e5 (test)
                            """
                        }
                    }
                }
            }
        }

        stage('Deploy on Docker') {
            steps {
                script {
                    if (env.BRANCH_NAME == "master") {
                        sh "docker rm -f masterapp || true"
<<<<<<< HEAD
                        sh "docker run -itd --name masterapp -p 8010:80 vijay3639/masterimage:latest"
                    } else if (env.BRANCH_NAME == "developer") {
                        sh "docker rm -f devapp || true"
                        sh "docker run -itd --name devapp -p 8020:80 vijay3639/devimage:latest"
=======
                        sh "docker run -itd --name masterapp -p 8010:80 meganathanu/masterimage:latest"
                    } else if (env.BRANCH_NAME == "developer") {
                        sh "docker rm -f devapp || true"
                        sh "docker run -itd --name devapp -p 8020:80 meganathanu/devimage:latest"
>>>>>>> 5ab40e5 (test)
                    }
                }
            }
        }

        stage('Cleanup Old Images') {
            steps {
                script {
                    if (env.BRANCH_NAME == "master") {
                        sh """
<<<<<<< HEAD
                            docker images "vijay3639/masterimage" --format "{{.Repository}}:{{.Tag}}" \
=======
                            docker images "meganathanu/masterimage" --format "{{.Repository}}:{{.Tag}}" \
>>>>>>> 5ab40e5 (test)
                            | grep -v "latest" \
                            | grep -v "${BUILD_NUMBER}" \
                            | xargs -r docker rmi -f
                        """
                    } else if (env.BRANCH_NAME == "developer") {
                        sh """
<<<<<<< HEAD
                            docker images "vijay3639/devimage" --format "{{.Repository}}:{{.Tag}}" \
=======
                            docker images "meganathanu/devimage" --format "{{.Repository}}:{{.Tag}}" \
>>>>>>> 5ab40e5 (test)
                            | grep -v "latest" \
                            | grep -v "${BUILD_NUMBER}" \
                            | xargs -r docker rmi -f
                        """
                    }

                    // Clean dangling layers also
                    sh "docker image prune -f"
                }
            }
        }
    }
}
