// A list of the directories which contain Dockerfiles.
// Each is to be built into a Docker image.
List<String> DOCKER_PROJECTS = ["gsad", "gvmd", "gvm-postgres", "openvas"]
String DOCKER_REGISTRY_URL = "https://303925148639.dkr.ecr.us-west-2.amazonaws.com"
String AWS_REGION = "us-west-2"  // Should we have an ap-southeast-2 repo? Maybe not...

// This is a closure to capture our globals above. Groovy scoping is weird...
def build_and_publish_container = { String project ->
    stage("Build and publish container: " + project) {
        environment { AWS_DEFAULT_REGION = AWS_REGION }
        try {
            sh "aws --region ${env.AWS_DEFAULT_REGION} ecr create-repository --repository-name ${project}"
            echo "AWS ECR Repository ${project} was created!"
        } catch(_) {
            echo "AWS ECR Repository ${project} already exists - skipping creation."
        }
        docker.withRegistry(DOCKER_REGISTRY_URL) {
            String dockerfile_path = [project, "Dockerfile"].join("/")

            // docker build -f openvas/Dockerfile openvas
            String build_args = ["-f", dockerfile_path, project].join(" ")
            sh """
                set +x
                \$(aws ecr get-login --no-include-email --region ${env.AWS_DEFAULT_REGION})
            """
                def image = docker.build("${project}:${env.BRANCH_NAME}-${env.BUILD_ID}", build_args)
                image.push()
        }
    }
}

pipeline {
    agent { label "ec2-docker-spot-t3-micro" }
    stages {
        stage("Checkout SCM") {
            steps {
                checkout scm
            }
        }
        stage("Build and publish containers") {
            steps {
                script {
                    DOCKER_PROJECTS.each { project ->
                        build_and_publish_container(project)
                    }
                }
            }
        }
    }
}
