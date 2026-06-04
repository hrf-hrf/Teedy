pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    stages {
        stage('Maven编译打包') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('PMD代码检查') {
            steps {
                sh 'mvn pmd:pmd'
            }
            post { always { pmd pattern: 'target/pmd.xml' } }
        }
        stage('执行单元测试') {
            steps {
                sh 'mvn test'
            }
        }
        stage('生成测试报告') {
            steps {
                sh 'mvn surefire-report:report'
            }
            post { always { html publishDir:'target/site',filename:'surefire-report.html',reportName:'单元测试报告' } }
        }
        stage('生成JavaDoc') {
            steps {
                sh 'mvn javadoc:jar'
            }
        }
    }
    post {
        success { archiveArtifacts artifacts:'target/*.jar,target/site/**,target/apidocs/**' }
    }
}