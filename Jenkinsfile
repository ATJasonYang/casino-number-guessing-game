pipeline {
    agent any

        // 👇 添加这两行，让 Jenkins 每隔 1 分钟检查一次 GitHub
    triggers {
        pollSCM '* * * * *'
    }
    
    stages {
        stage('Build') {
            steps {
                bat 'if exist build rmdir /s /q build'      // 删除 build 目录
                bat 'cmake -B build -S .'                   // 配置 CMake
                bat 'cmake --build build'                   // 编译项目
            }
        }
        stage('Test') {
            steps {
                bat '.\\build\\Debug\\casino_game.exe'              // 运行主程序
                bat '.\\build\\Debug\\test_game.exe'                // 运行测试程序
                echo 'Auto triggered successfully!'
            }
        }
        stage('Deliver') {
            steps {
                bat 'tar -czf casino_game.tar.gz build\\Debug\\casino_game.exe'  // 打包 exe 文件
                archiveArtifacts artifacts: 'casino_game.tar.gz', fingerprint: true
            }
        }
    }
}