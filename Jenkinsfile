
pipeline {
    environment {
        registry = "mnkap/python-jenkins" //To push an image to Docker Hub, you must first name your local image using your Docker Hub username and the repository name that you created through Docker Hub on the web.
        registryCredential = 'gbt1'
        githubCredential = 'mnkap'
        VIRTUAL_ENV = '/var/lib/jenkins/pytest_env'
    }
    agent any
    stages {
        
        stage('checkout') {
                steps {
                git branch: 'main',
                credentialsId: githubCredential,
                url: 'https://github.com/mnkap/jenkins.git'
                }
        }
   
        stage('Set up Virtual Environment') {
            steps {
                script {
                    sh '''
                       
                        sudo python3 -m venv $VIRTUAL_ENV
                        sudo bash -c '$VIRTUAL_ENV/bin/activate && sudo pip install pytest'
                        
                    '''
                }
            }
        }
   
        stage ('Test'){
                steps {
                    script {
                    // Activate the virtual environment and run pytest
                    sh '''
                        . $VIRTUAL_ENV/bin/activate  # Use dot (.) again
                        sudo pytest testRoutes.py
                    '''
                    }
        }
        }
        
        stage ('Clean Up'){
            steps{
                sh returnStatus: true, script: 'docker stop $(docker ps -a | grep ${JOB_NAME} | awk \'{print $1}\')'
                sh returnStatus: true, script: 'docker rm ${JOB_NAME}'
            }
        }

        stage('Build Image') {
            steps {
                script {
                    img = registry + ":${env.BUILD_ID}"
                    println ("${img}")
                    dockerImage = docker.build("${img}")
                }
            }
        }

        stage('Push To DockerHub') {
            steps {
                script {
                    docker.withRegistry( 'https://registry.hub.docker.com ', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
        }
                    
        stage('Deploy') {
           steps {
                sh label: '', script: "docker run -d --name ${JOB_NAME} -p 5000:5000 ${img}"
          }
        }

      }
    }
