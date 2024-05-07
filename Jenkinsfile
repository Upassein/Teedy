pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                // 编译代码
                sh 'mvn -B -DskipTests clean package'
            }
        }
//         stage('Test') {
//             steps {
//                 // 运行测试并生成 Surefire 报告
//                 sh 'mvn test'
//                 // 生成 Surefire 报告
//                 sh 'mvn surefire-report:report'
//             }
//         }
        stage('Test') {
            steps {
                script {
                    try {
                        // 运行测试并生成 Surefire 报告
                        sh 'mvn test'
                        // 生成 Surefire 报告
                        sh 'mvn surefire-report:report'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE' // 将构建结果设置为失败
                        echo "Tests failed: ${e.message}" // 输出错误信息
                    }
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
