
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
                        . /var/lib/jenkins/pytest_env/bin/activate
                        sudo /var/lib/jenkins/pytest_env/bin/pip install pytest

                        
                    '''
                }
            }
        }
   
        stage ('Test'){
                steps {
                    script {
                    // Activate the virtual environment and run pytest
                    sh '''
                        . /var/lib/jenkins/pytest_env/bin/activate  # Use dot (.) again
                        sudo pytest testRoutes.py
                    '''
                    }
        }
        }
        

        stage('Build Image') {
            steps {
          script {
                    def img = "${registry}"
                    echo "Building Docker image: ${img}"
                    sh "sudo docker build -t ${img} -f ${dockerfile} ${context}"
                }
            }
        }

        stage('Push To DockerHub') {
            steps {
                script {
                    sudo docker.withRegistry( 'https://registry.hub.docker.com ', registryCredential ) {
                        sudo dockerImage.push()
                    }
                }
            }
        }
                    
        stage('Deploy') {
           steps {
                sh label: '', script: "sudo docker run -d --name ${JOB_NAME} -p 5000:5000 ${img}"
          }
        }

      }
    }
