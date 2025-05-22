@Library('jenkins-shared-library-examples@main') _

pipeline {
    agent any

    parameters {
        booleanParam(name: 'RUN_SONARQUBE', defaultValue: true, description: 'Run SonarQube scan')
        booleanParam(name: 'RUN_TRIVY', defaultValue: true, description: 'Run Trivy scan')
    }

    stages {
        stage('Fetch Terraform Output') {
            steps {
                script {
                    step([
                        $class: 'CopyArtifact',
                        projectName: 'Terraform-Infra-Pipeline',
                        filter: 'tf_outputs.json',
                        target: 'terraform-data',
                        flatten: true,
                        selector: [$class: 'StatusBuildSelector', stable: false]
                    ])
                }
            }
        }

        stage('Run CI Pipeline') {
            steps {
                script {
                    ciPipeline(terraformOutputFile: 'terraform-data/tf_outputs.json')
                }
            }
        }
    }
}
