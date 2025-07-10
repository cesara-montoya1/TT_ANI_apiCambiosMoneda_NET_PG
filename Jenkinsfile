pipeline {
    agent any

    environment {
        CONTAINER_IMAGE = 'monedas-net-api'
        CONTAINER_NAME = 'monedas-net-api'
        CONTAINER_NETWORK = 'monedas_network'
        CONTAINER_PORT = '5235'
        HOST_PORT = '5235'
    }

    stages {
        stage('Unit tests') {
            steps {
                sh 'dotnet test apiCambiosMoneda.Test/apiCambiosMoneda.Test.csproj -c Release'
            }
        }

        stage('Build Docker image') {
            steps {
                sh "podman build -t $CONTAINER_IMAGE ."
            }
        }

        stage('Clear existing containers') {
            steps {
                sh "podman container rm -f $CONTAINER_NAME"
            }
        }

        stage('Ship container') {
            steps {
                sh "podman run --name ${CONTAINER_NAME} --network ${CONTAINER_NETWORK} -p ${HOST_PORT}:${CONTAINER_PORT} -d ${CONTAINER_IMAGE}"
            }
        }
    }
}
