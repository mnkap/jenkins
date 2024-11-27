pipeline {
    environment {
        registry = "python-jenkins" //To push an image to Docker Hub, you must first name your local image using your Docker Hub username and the repository name that you created through Docker Hub on the web.
        registryCredential = 'gbt1'
        tag = 'latest'
        githubCredential = 'mnkap'
        VIRTUAL_ENV = '/var/lib/jenkins/pytest_env'
        dockerfile = 'Dockerfile'
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
                    sh "sudo docker build -t ${img} -f ${dockerfile} ."
                }
            }
        }

        stage('Push To DockerHub') {
            steps {
                
                 docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        dockerImage.push()
                    
                }
            }
        
                    
        stage('Deploy') {
           steps {
                sh "sudo docker run -d --name ${JOB_NAME} -p 5000:5000 ${img}"
          }
        }

    }
    }
