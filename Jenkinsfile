// Groovy (Jenkins Pipeline) 스크립트 시작
pipeline {
    agent any // 이 Jenkins VM 아무데서나 실행

    // 환경 변수 정의 (스크립트 전체에서 사용)
    environment {
        DOCKERHUB_ID = 'mudd0'
        IMAGE_NAME = 'django_k8s_project'
        // 새 버전 태그 (예: v1.1, v1.2, v1.3 ...)
        // BUILD_NUMBER는 Jenkins가 자동으로 1, 2, 3... 증가시켜줍니다.
        IMAGE_TAG = "v1.${BUILD_NUMBER}"
    }

    // CI/CD '단계' 정의
    stages {
        
        // 1단계: Docker 이미지 빌드
        stage('Build Docker Image') {
            steps {
                echo "1. Docker 이미지 빌드를 시작합니다: ${DOCKERHUB_ID}/${IMAGE_NAME}:${IMAGE_TAG}"
                sh "docker build -t ${DOCKERHUB_ID}/${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        // 2단계: Docker Hub 로그인 및 Push
        stage('Push to Docker Hub') {
            steps {
                echo "2. Docker Hub에 로그인 및 PUSH를 시작합니다."
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh "echo ${DOCKER_PASS} | docker login -u ${DOCKER_USER} --password-stdin"
                    sh "docker push ${DOCKERHUB_ID}/${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }

        // 3단계: (정리) VM에 쌓이는 이미지 삭제
        stage('Clean up Docker Image') {
            steps {
                echo "3. 빌드에 사용된 로컬 이미지를 삭제합니다."
                sh "docker rmi ${DOCKERHUB_ID}/${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }
    }
}
// 스크립트 끝
