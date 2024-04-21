pipeline
{
    agent any
    
    stages
    {
        stage("code")
        {
            steps
            {
                echo "cloning the code from github repository"
                git url:"https://github.com/systemuet/django-notes-app.git", branch: "main"
            }
        }
        
        stage("build")
        {
            steps
            {
                echo "building the docker image"
                sh "docker build -t node-app ."
            }
        }
        
        stage("push to dockerhub")
        {
            steps
            {
                echo "pushing docker image to dockerhub"
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubpassword",usernameVariable:"dockerhubuser")])
                {
                    sh "docker tag node-app ${env.dockerhubuser}/cicd-pipeline:node-app"
                    sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpassword}"
                    sh "docker push ${env.dockerhubuser}/cicd-pipeline:node-app"
                }
                
            }
        }
        
        stage("deploy")
        {
            steps
            {
                echo "deploying the app at server"
                sh "docker-compose up -d"
            }
        }
    }
    
}
