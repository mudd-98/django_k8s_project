stage('Update K8s Manifest') {
    steps {
        echo "3. K8s YAML 파일의 이미지 태그를 ${IMAGE_TAG}로 업데이트합니다."
        withCredentials([usernamePassword(credentialsId: 'github-push-token', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_TOKEN')]) {
            sh "git config --global user.email 'jenkins@ci.cd'"
            sh "git config --global user.name 'Jenkins-Bot'"
            sh "git clone https://${GIT_USER}:${GIT_TOKEN}@github.com/mudd-98/django_k8s_project.git temp_manifest_repo"
            dir('temp_manifest_repo') {
                sh "sed -i 's|image: mudd-98/django_k8s_project:.*|image: ${DOCKERHUB_ID}/${IMAGE_NAME}:${IMAGE_TAG}|g' k8s/08-web-deployment.yaml"
                sh "git add k8s/08-web-deployment.yaml"
                sh "git commit -m 'CI: Update image tag to ${IMAGE_TAG}'"
                sh "git push origin master"
            }
        }
    }
}
