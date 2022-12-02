pipeline {
  agent any
  stages {
    stage('Clean WorkSpace') {
      steps {
        cleanWs(cleanWhenAborted: true, cleanWhenFailure: true, cleanWhenNotBuilt: true, cleanWhenSuccess: true, cleanWhenUnstable: true, deleteDirs: true)
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/q3x4k3p7'
        sh 'docker build -t dotnet .'
        sh 'docker tag dotnet:latest public.ecr.aws/q3x4k3p7/dotnet:latest'
        sh 'docker push public.ecr.aws/q3x4k3p7/dotnet:latest'
      }
    }

    stage('Copyfilesfrom S3') {
      steps {
        s3Download(file: 'deployment.yaml', bucket: 's3://sam-bucket-jnks', path: './src')
        s3Download(file: 'service.yaml', bucket: 's3://sam-bucket-jnks', path: './src')
      }
    }

  }
}