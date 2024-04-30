pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                // 编译代码
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                // 运行测试并生成 Surefire 报告
                sh 'mvn test'
                // 生成 Surefire 报告
                sh 'mvn surefire-report:report'
            }
            post {
                // 将 Surefire 报告作为构建产物进行归档
                always {
                    archiveArtifacts artifacts: '**/target/surefire-reports/*', fingerprint: true
                }
            }
        }
        stage('PMD') {
            steps {
                // 运行 PMD 静态代码分析
                sh 'mvn pmd:pmd'
            }
        }
        stage('Generate Javadoc') {
            steps {
                // 生成 Javadoc
                sh 'mvn javadoc:jar'
            }
            post {
                // 将 Javadoc 作为构建产物进行归档
                always {
                    archiveArtifacts artifacts: '**/target/site/*', fingerprint: true
                }
            }
        }
    }
    
    post {
        always {
            // 归档其他构建产物（JAR 和 WAR 文件）
            archiveArtifacts artifacts: '**/target/**/*.jar', fingerprint: true
            archiveArtifacts artifacts: '**/target/**/*.war', fingerprint: true
            archiveArtifacts artifacts: '**/target/site/**', fingerprint: true
        }
    }
}
