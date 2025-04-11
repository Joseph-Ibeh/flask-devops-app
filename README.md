# Building and Automating the Deployment of a Flask Application with Docker, GitHub, and CI/CD

## 📚 Introduction

This project demonstrates how to build, containerize, and deploy a simple Flask application with Docker, using GitHub for version control and GitHub Actions for Continuous Integration/Continuous Deployment (CI/CD). You will learn how to:

- Create a simple Flask web application.
- Containerize the application using Docker.
- Set up version control with Git and GitHub.
- Implement CI/CD pipelines using GitHub Actions to build and deploy the Docker image.

## ⚙️ Prerequisites

Before you begin, ensure that you have the following tools installed:

- **Python**: A programming language for building web applications.
- **Flask**: A lightweight Python web framework.
- **Docker**: A platform for developing, shipping, and running applications in containers.
- **Git**: A version control system for tracking changes in code.
- **GitHub**: A platform for hosting and sharing code repositories.
- **Docker Hub**: A cloud-based registry to share Docker images.
  
If you don't have these installed, refer to their respective official websites for installation instructions.

---

## ✅ Step 1: Create a Simple Flask Application

1. **Create a new folder for the project**:
    ```bash
    mkdir flask-devops-app
    cd flask-devops-app
    ```
![mkdir & cd](https://github.com/Joseph-Ibeh/flask-devops-app/blob/main/Assets/mkdir_cd%201.png)


2. **Set up a Python virtual environment**:
    ```bash
    python -m venv venv
    ```

3. **Activate the virtual environment**:
    ```bash
    source venv/Scripts/activate  # On Windows use: venv\Scripts\activate
    ```

4. **Install Flask**:
    ```bash
    pip install flask
    ```
![install flask](https://github.com/Joseph-Ibeh/flask-devops-app/blob/main/Assets/venv_install_flask_2.png)

![mkdir & cd](https://github.com/Joseph-Ibeh/flask-devops-app/blob/main/Assets/venv_2_contd.png)

5. **Create the `app.py` file**:
    ```python
    # app.py
    from flask import Flask

    app = Flask(__name__)

    @app.route('/')
    def hello():
        return "Hello DevOps!"

    if __name__ == '__main__':
        app.run(host='0.0.0.0', port=5000)
    ```
![app.py ](https://github.com/Joseph-Ibeh/flask-devops-app/blob/main/Assets/app.%20py%20_3.png)
6. **Run the app locally**:
    ```bash
    python app.py
    ```

7. Visit `http://localhost:5000` in your browser. You should see `"Hello DevOps!"`.

![test ](https://github.com/Joseph-Ibeh/flask-devops-app/blob/main/Assets/flask%20running%203.png)
---

## ✅ Step 2: Containerize the Application

1. **Create a `Dockerfile` in the same folder**:
    ```dockerfile
    # Dockerfile
    FROM python:3.9-slim

    WORKDIR /app

    COPY requirements.txt requirements.txt
    RUN pip install -r requirements.txt

    COPY . .

    CMD ["python", "app.py"]
    ```
![Dockerfile](https://github.com/Joseph-Ibeh/flask-devops-app/blob/main/Assets/dockerfile%204.png)
2. **Generate `requirements.txt`**:
    ```bash
    pip freeze > requirements.txt
    ```

3. **Create a `.dockerignore` file**:
    ```
    __pycache__/
    *.pyc
    venv/
    ```
![Docker ignore](https://github.com/Joseph-Ibeh/flask-devops-app/blob/main/Assets/dockerignorefile%205.png)
4. **Build the Docker image**:
    ```bash
    docker build -t flask-devops-app .
    ```
![Dockerbuild](https://github.com/Joseph-Ibeh/flask-devops-app/blob/main/Assets/dockerbuild%206.png)
5. **Run the Docker container**:
    ```bash
    docker run -p 5000:5000 flask-devops-app
    ```

6. Open `http://localhost:5000` again and see your app running inside a Docker container.
![run](https://github.com/Joseph-Ibeh/flask-devops-app/blob/main/Assets/docker%20running%207.png)
---

## ✅ Step 3: Set Up Version Control

### 🧰 Tools: Git + GitHub

1. **Initialize a Git repository**:
    ```bash
    git init
    ```

2. **Create a `.gitignore` file**:
    ```
    venv/
    __pycache__/
    *.pyc
    ```

![git init, ignore etc ](https://github.com/Joseph-Ibeh/flask-devops-app/blob/main/Assets/gitinit_gitignore%208.png)

3. **Commit your files**:
    ```bash
    git add .
    git commit -m "Initial commit"
    ```

4. **Create a GitHub repository and link it to your local repo**:
    ```bash
    git remote add origin https://github.com/YOUR_USERNAME/flask-devops-app.git
    git branch -M main
    git push -u origin main
    ```
![remote add](https://github.com/Joseph-Ibeh/flask-devops-app/blob/main/Assets/git%20remote%20add-%20branch-push%209.png)
---

## ✅ Step 4: Implement CI/CD with GitHub Actions

### 🧰 Tools: GitHub Actions, Docker Hub

1. **Create a `.github/workflows/docker-publish.yml` file**:
    ```bash
    mkdir -p .github/workflows
    touch .github/workflows/docker-publish.yml
    ```

2. **Edit the `docker-publish.yml` file (vim docker-publish.yml) ** and add the following content:
    ```yaml
    # .github/workflows/docker-publish.yml
    name: Build and Push Docker Image

    on:
      push:
        branches: [ main ]

    jobs:
      build:
        runs-on: ubuntu-latest

        steps:
          - name: Checkout code
            uses: actions/checkout@v3

          - name: Set up Docker Buildx
            uses: docker/setup-buildx-action@v2

          - name: Log in to Docker Hub
            uses: docker/login-action@v2
            with:
              username: ${{ secrets.DOCKER_USERNAME }}
              password: ${{ secrets.DOCKER_PASSWORD }}

          - name: Build and push image
            uses: docker/build-push-action@v5
            with:
              context: .
              push: true
              tags: YOUR_DOCKERHUB_USERNAME/flask-devops-app:latest
    ```
![workflow file](https://github.com/Joseph-Ibeh/flask-devops-app/blob/main/Assets/git-docker-workflowyml%20file%2010.png)

3. **Add Docker Hub credentials as GitHub Secrets**:
   - Go to your GitHub repository → **Settings** → **Secrets and variables** → **Actions**
   - Add the following secrets:
     - `DOCKER_USERNAME` → your Docker Hub username
     - `DOCKER_PASSWORD` → your Docker Hub password or access token
    
    ![dockerhub credential as github secret](https://github.com/Joseph-Ibeh/flask-devops-app/blob/main/Assets/docker-cedential-github-secret%2011.png)

4. **Push your code again**:
    ```bash
    git add .
    git commit -m "Add CI/CD workflow"
    git push
    ```

GitHub Actions will now automatically build your Docker image and push it to Docker Hub whenever you push to the `main` branch.

---

## 🚀 Final Output

- ✅ A live Flask app running inside a Docker container.
- ✅ The source code is hosted on GitHub.
- ✅ The Docker image is automatically built and pushed to Docker Hub on every commit.

![Triggered](https://github.com/Joseph-Ibeh/flask-devops-app/blob/main/Assets/build%20and%20push%20image%20successfuly%2012.png)
---

## 🎯 Conclusion

Congratulations! You've successfully created a simple Flask app, containerized it with Docker, set up version control with Git and GitHub, and implemented CI/CD with GitHub Actions. This project is a foundational step in building scalable and automated systems in DevOps.

---

## 🛠️ Troubleshooting

- **If Docker is not running**: Ensure Docker Desktop is open and running.
- **If you encounter permission issues with GitHub Actions**: Double-check your Docker Hub credentials in GitHub Secrets.
- **If the Flask app does not load**: Ensure you are using the correct port and that the Docker container is running.


