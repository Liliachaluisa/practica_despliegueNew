pipeline {
    agent any

    tools {
        nodejs "Node25"
        dockerTool "Dockertool" 
    }

    stages {
        stage('Instalar dependencias') {
            steps {
                sh 'npm install'
            }
        }

        stage('Ejecutar tests') {
            steps {
                sh '''
                    chmod +x node_modules/.bin/jest
                    npx jest
                '''
            }
        }

        stage('Construir Imagen Docker') {
            when {
                expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
            steps {
                sh 'docker build -t hola-mundo-node:latest .'
            }
        }

        stage('Ejecutar Contenedor Node.js') {
    steps {
        sh '''
            docker ps -q --filter "name=hola-mundo-node" | xargs -r docker stop
            docker ps -aq --filter "name=hola-mundo-node" | xargs -r docker rm

            docker run -d --name hola-mundo-node -p 3000:3000 hola-mundo-node:latest
        '''
    }
}

    }
}