pipeline {
  agent any

  environment {
    TF_DIR = "sree/gke"
    PROJECT_ID = "project-135912bf-7758-481f-965"
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/sreeaol/gke.git'
      }
    }

    stage('Terraform Init') {
      steps {
        dir("${TF_DIR}") {
          sh 'terraform init'
        }
      }
    }

    stage('Terraform Plan') {
      steps {
        dir("${TF_DIR}") {
          sh 'terraform plan -out=tfplan'
        }
      }
    }

    stage('Terraform Apply') {
      steps {
        dir("${TF_DIR}") {
          sh 'terraform apply -auto-approve tfplan'
        }
      }
    }

    stage('Configure kubectl') {
      steps {
        sh "gcloud container clusters get-credentials ${env.CLUSTER_NAME} --region ${env.REGION} --project ${PROJECT_ID}"
        sh "kubectl get nodes"
      }
    }
  }

  post {
    success {
      echo "✅ GKE cluster provisioned successfully!"
    }
    failure {
      echo "❌ Pipeline failed. Check logs."
    }
  }
}
