pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
    }

    stages {

        stage("Cloning from Github....") {
            steps {
                script {
                    echo 'Cloning from Github...'
                    checkout scmGit(
                        branches: [[name: '*/main']],
                        extensions: [],
                        userRemoteConfigs: [[
                            credentialsId: 'github-token', 
                            url: 'https://github.com/swasti-jain19/Hybrid_Anime_Recommender_System.git'
                        ]]
                    )
                }
            }
        }

        stage("Making a virtual environment....") {
            steps {
                script {
                    echo 'Making a virtual environment...'
                    sh """
                    python -m venv ${VENV_DIR} \
                    && . ${VENV_DIR}/bin/activate \
                    && pip install --upgrade pip \
                    && pip install -e . \
                    && pip install dvc
                    """
                }
            }
        }

        stage('DVC Pull') {
            steps {
                withCredentials([file(credentialsId:'gcp-key', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
                    script {
                        echo 'DVC Pull....'
                        sh """
                        . ${VENV_DIR}/bin/activate \
                        && dvc pull
                        """
                    }
                }
            }
        }
    }
}
