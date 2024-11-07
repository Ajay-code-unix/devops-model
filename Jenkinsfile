   pipeline {
       agent any
       triggers {
        pollSCM('* * * * *') // Poll SCM every minute for changes
    }
       stages {
           stage('Clone Repository') {
               steps {
                   // Cloning the Git repository
                   
                   checkout([$class: 'GitSCM', branches: [[name: '*/staging'],[name:'*/production']],
                   userRemoteConfigs: [[url: 'https://github.com/Ajay-code-unix/devops-model.git']]
                   ])
               }
           }
           stage('Build Docker Image') {
               steps {
                   script {
                       sh "git checkout staging"
                       sh "sudo docker build --no-cache  -t ajaybadrinath/staging:latest ."
                       sh "git checkout production"
                        sh "sudo docker build --no-cache  -t ajaybadrinath/prod:latest ."
                   }
               }
           }
           stage('Push Docker Image') {
               steps {
                   
                       sh "echo 'ajay@2002' | sudo -S docker login -u ajaybadrinath --password-stdin"
                       sh  'sudo docker push ajaybadrinath/staging:latest'
                       sh  'sudo docker push ajaybadrinath/prod:latest'
                       
                   
               }
           }
           stage('Deploy to Staging') {
               steps {
                   script{
                  // sh 'sudo docker rm -f blue || true'
                   //sh 'sudo docker run -d --name blue -p 3001:3000 ajaybadrinath/bg:latest'
                   
                   deployToStaging()
               }
           }}
            stage('Deploy to Prod') {
               steps {
                   script{
                  // sh 'sudo docker rm -f blue || true'
                   //sh 'sudo docker run -d --name blue -p 3001:3000 ajaybadrinath/bg:latest'
                   
                   deployToProd()
               }
           }}
       }
       
        

}
def deployToStaging() {
    sh 'sudo docker rm -f staging-container || true'
    sh 'sudo docker run -d --name staging-container -p 3005:3000 ajaybadrinath/staging:latest'
}

def deployToProd() {
    sh 'sudo docker rm -f prod-container || true'
    sh 'sudo docker run -d --name prod-container -p 3006:3000 ajaybadrinath/prod:latest'
}
