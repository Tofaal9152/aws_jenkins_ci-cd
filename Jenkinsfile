node {
    def APPDIR = "/var/www/nextjs-app"
    
    stage('Cleaning Workspace') {
        echo "Cleaning workspace..."
        deleteDir()
    }

    stage('Cloning Repository') {
        echo "Cloning repository..."
        git (
            branch: 'main',
            url: 'https://github.com/Tofaal9152/aws_jenkins_ci-cd.git'
        )
    }

    stage('Deploying to EC2') {
        echo "Deploying to EC2 instance..."
        sh """
            sudo mkdir -p ${APPDIR}
            sudo  chown -R jenkins:jenkins ${APPDIR}    

            rsync -av --delete --exclude='.git' --exclude='node_modules' ./ ${APPDIR}

            cd ${APPDIR}
            npm install
            npm run build
            sudo fuser -k 3000/tcp || true
            npm start
        """
    }
}