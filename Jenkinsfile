// Declarative pipelines must be enclosed with a "pipeline" directive.
pipeline {
	
    // This line is required for declarative pipelines. Just keep it here.
    agent any
    tools {
	jdk 'jdk1.8'
	maven 'M3'
    }

    // This section contains environment variables which are available for use in the
    // pipeline's stages.
    environment {
	    //JAVA_HOME="/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.242.b08-0.50.amzn1.x86_64"
	    region = "us-east-1"
        docker_repo_uri = "819314761995.dkr.ecr.us-east-1.amazonaws.com/sample-app"
		task_def_arn = "arn:aws:ecs:us-east-1:819314761995:task-definition/first-run-task-definition"
        cluster = "sample-app"
        exec_role_arn = "arn:aws:iam::819314761995:role/ecsTaskExecutionRole"
    }
    
    // Here you can define one or more stages for your pipeline.
    // Each stage can execute one or more steps.
   stages{
    stage('Build') {
     steps {
		echo 'Building..'
	sh "echo $PATH"
	
        sh "mvn clean install"
               
        // Get SHA1 of current commit
        script {
            commit_id = sh(script: "git rev-parse --short HEAD", returnStdout: true).trim()
        }
        // Build the Docker image
        sh "docker build -t ${docker_repo_uri}:${commit_id} ."
        // Get Docker login credentials for ECR
        sh "/usr/local/bin/aws ecr get-login --no-include-email --region ${region} | sh"
        // Push Docker image
        sh "docker push ${docker_repo_uri}:${commit_id}"
        // Clean up
        sh "docker rmi -f ${docker_repo_uri}:${commit_id}"
     }
  }
  stage('Deploy') {
    steps {
        // Override image field in taskdef file
        sh "sed -i 's|{{image}}|${docker_repo_uri}:${commit_id}|' taskdef.json"
        // Create a new task definition revision
        sh "/usr/local/bin/aws ecs register-task-definition --execution-role-arn ${exec_role_arn} --cli-input-json file://taskdef.json --region ${region}"
        // Update service on Fargate
        sh "/usr/local/bin/aws ecs update-service --cluster ${cluster} --service sample-app-service --task-definition ${task_def_arn} --region ${region}"
    }
  }
 }
}
