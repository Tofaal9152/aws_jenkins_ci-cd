// For nextjs app deployment on EC2 using Jenkins, you can use the following Jenkinsfile. This pipeline will clean the workspace, clone the repository, and deploy the Next.js application to the EC2 instance.

// node {
//     def APPDIR = "/var/www/nextjs-app"
    
//     stage('Cleaning Workspace') {
//         echo "Cleaning workspace..."
//         deleteDir()
//     }

//     stage('Cloning Repository') {
//         echo "Cloning repository..."
//         git (
//             branch: 'main',
//             url: 'https://github.com/Tofaal9152/aws_jenkins_ci-cd.git'
//         )
//     }

//     stage('Deploying to EC2') {
//         echo "Deploying to EC2 instance..."
//         sh """
//             sudo mkdir -p ${APPDIR}
//             sudo  chown -R jenkins:jenkins ${APPDIR}    

//             rsync -av --delete --exclude='.git' --exclude='node_modules' ./ ${APPDIR}

//             cd ${APPDIR}
//             npm install
//             npm run build
//             sudo fuser -k 3000/tcp || true
//             npm start
//         """
//     }
// }

// For nextjs Docker deployment on EC2 using Jenkins, you can use the following Jenkinsfile. This pipeline will clean the workspace, clone the repository, build the Docker image, and deploy the Next.js application to the EC2 instance using Docker. i add both pipeline and node
// pipeline {
//     agent any

//     environment {
//         CONTAINER_NAME = "nextjs-app"
//         IMAGE_NAME = "nextjs-app-image"
//         EMAIL = "tofaal91522@gmail.com"
//         HOST_PORT = "4000"     // Apnar server-er port
//         CONTAINER_PORT = "3000" // Next.js er default port
//     }

//     stages {
//         stage('Cloning Repository') {
//             steps {
//                 echo "Cloning repository..."
//                 git branch: 'main', url: 'https://github.com/Tofaal9152/aws_jenkins_ci-cd.git'
//             }
//         }
        
//         stage('Building Docker Image') {
//             steps {
//                 echo "Building Docker image..."
//                 // Ekdom sesher dot (.) ta khub important!
//                 sh 'docker build -t ${IMAGE_NAME} .' 
//             }
//         }
        
//         stage('Stopping Existing Container') {
//             steps {
//                 echo "Stopping and removing existing container..."
//                 sh """
//                     docker stop ${CONTAINER_NAME} || true
//                     docker rm ${CONTAINER_NAME} || true
//                 """
//             }
//         }
        
//         stage('Run Docker Container') {
//             steps {
//                 echo "Running new container..."
//                 sh 'docker run -d -p ${HOST_PORT}:${CONTAINER_PORT} --name ${CONTAINER_NAME} ${IMAGE_NAME}'
//             }
//         }
//     }
    
//     // Deployment success holei shudhu email pathabe
//     post {
//         success {
//             echo "Sending email notification..."
//             mail to: "${EMAIL}",
//                  subject: "Next.js App Deployment Success",
//                  body: "The Next.js application has been deployed successfully to the EC2 instance. http://16.170.250.202:${HOST_PORT}/"
//         }
//         failure {
//             echo "Deployment failed! Sending failure email..."
//             mail to: "${EMAIL}",
//                  subject: "Next.js App Deployment FAILED",
//                  body: "The deployment pipeline failed. Please check Jenkins logs."
//         }
//     }
// }

node {
    def CONTAINER_NAME = "nextjs-app"
    def IMAGE_NAME = "nextjs-app-image"
    def EMAIL = "tofaal91522@gmail.com"
    def HOST_PORT = "3000"     // Apnar server-er port
    def CONTAINER_PORT = "3000" // Next.js er default port

    try {
        stage('Cleaning Workspace') {
            echo "Cleaning workspace..."
            deleteDir() // Agam kachra clean kora valo
        }

        stage('Cloning Repository') {
            echo "Cloning repository..."
            git (
                branch: 'main',
                url: 'https://github.com/Tofaal9152/aws_jenkins_ci-cd.git'
            )
        }
        
        stage('Building Docker Image') {
            echo "Building Docker image..."
            sh "docker build -t ${IMAGE_NAME} ."
        }
        
        stage('Stopping Existing Container') {
            echo "Stopping and removing existing container..."
            sh """
                docker stop ${CONTAINER_NAME} || true
                docker rm ${CONTAINER_NAME} || true
            """
        }
        
        stage('Run Docker Container') {
            echo "Running new container..."
            sh "docker run -d -p ${HOST_PORT}:${CONTAINER_PORT} --name ${CONTAINER_NAME} ${IMAGE_NAME}"
        }

        stage('Send Success Email') {
            echo "Sending email notification..."
            mail (
                to: "${EMAIL}",
                subject: "Next.js App Deployment Success",
                body: "The Next.js application has been deployed successfully to the EC2 instance. http://16.170.250.202:${HOST_PORT}/"
            )
        }

    } catch (Exception e) {
        // Jodi uporer kono ekta stage-e error ashe, tahole ekhane chole ashbe
        stage('Send Failure Email') {
            echo "Deployment failed! Sending failure email..."
            mail (
                to: "${EMAIL}",
                subject: "Next.js App Deployment FAILED",
                body: "The deployment pipeline failed. Please check Jenkins logs."
            )
        }
        // Pipeline take officially "Failed" mark korar jonno
        throw e 
    }
}