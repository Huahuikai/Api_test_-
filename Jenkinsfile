pipeline {
    agent any

    stages {
        stage('执行接口测试') {
            steps {
                echo '========== 执行 Newman 测试 =========='
                bat '''
                    newman run ApiTest.postman_collection.json ^
                        -e crm-env.postman_environment.json ^
                        --delay-request 50 ^
                        --timeout-request 15000 ^
                        --reporters cli,htmlextra ^
                        --reporter-htmlextra-export newman-report.html ^
                        --reporter-htmlextra-title "CRM API Test Report"
                '''
            }
        }
    }

    post {
        always {
            publishHTML([
                allowMissing: true,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: '.',
                reportFiles: 'newman-report.html',
                reportName: 'CRM 接口测试报告',
                reportTitles: 'API Test Results'
            ])
        }
        success {
            echo '✓ 全部测试通过'
        }
        failure {
            echo '✗ 测试存在失败项，查看左侧报告'
        }
    }
}
