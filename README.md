# GitLab Pipeline
To upload the HTML file as a GitLab Pipeline Artifact, you can use the following GitLab CI/CD configuration file (`.gitlab-ci.yml`):

Place this `.gitlab-ci.yml` file in the root of your GitLab repository. When you push changes to your repository, GitLab CI/CD will automatically execute this pipeline:

1. In the `download_html` stage, it will use `curl` to download the HTML file and save it as `homepage.html`.

2. In the `upload_artifact` stage, it will upload the `homepage.html` file as a GitLab Pipeline Artifact, making it available for download in the GitLab UI or for use in subsequent stages.

Make sure to replace `"http://Your-AWS-Server-IP"` with the actual URL of your Ghost Site's homepage in both the `variables` section and the `download_html` stage.