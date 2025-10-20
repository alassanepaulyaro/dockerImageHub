# Docker CI/CD Pipeline with GitHub Actions

A complete CI/CD pipeline demonstration project that shows how to build, test, and automatically deploy a Flask application to Docker Hub using GitHub Actions.

## Project Overview

This project demonstrates a production-ready CI/CD workflow that:
- Builds a Flask web application
- Runs automated tests with pytest
- Creates Docker images
- Automatically publishes to Docker Hub on successful builds

## Project Structure

```
dockerImageHub/
├── .github/
│   └── workflows/
│       └── cicd.yml          # GitHub Actions CI/CD pipeline configuration
├── app.py                    # Flask application (main entry point)
├── test_app.py              # Pytest unit tests
├── Dockerfile               # Docker container configuration
├── requirements.txt         # Python dependencies
├── .gitignore              # Git ignore rules
└── README.md               # Project documentation
```

## Application Details

### Flask Application ([app.py](app.py))
A simple Flask web application that:
- Runs on port 8000
- Exposes a single endpoint (`/`) that returns "Hello World!"
- Configured to accept connections from any host (0.0.0.0)

### Tests ([test_app.py](test_app.py))
Automated tests using pytest that verify:
- HTTP 200 status code response
- Correct "Hello World!" message output

### Docker Configuration ([Dockerfile](Dockerfile))
Multi-stage Docker build that:
- Uses Python 3.9-slim base image
- Sets up working directory at `/app`
- Installs Flask dependencies
- Exposes port 8000
- Runs the Flask application on container startup

## CI/CD Pipeline

The GitHub Actions workflow ([.github/workflows/cicd.yml](.github/workflows/cicd.yml)) consists of three jobs:

### 1. Docker Build Job (`dockerbuild`)
- Checks out the code
- Builds a timestamped Docker image for validation
- Runs on: push to `main` branch or pull requests

### 2. Build and Test Job (`build-and-test`)
- Sets up Python 3.9 environment
- Installs dependencies (Flask, pytest)
- Runs automated tests with pytest
- Must pass before deployment

### 3. Build and Publish Job (`build-and-publish`)
- **Depends on**: Successful test completion
- Sets up Docker Buildx
- Authenticates with Docker Hub
- Builds and pushes Docker image with `latest` tag
- Published as: `<DOCKER_USERNAME>/flasktest-app:latest`

## Setup Instructions

### Prerequisites
- GitHub account
- Docker Hub account
- Git installed locally

### Local Development

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd dockerImageHub
   ```

2. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Run the application locally**
   ```bash
   python app.py
   ```
   Access at: http://localhost:8000

4. **Run tests**
   ```bash
   pytest test_app.py
   ```

### Docker Setup

1. **Build the Docker image**
   ```bash
   docker build -t flasktest-app:latest .
   ```

2. **Run the container**
   ```bash
   docker run -p 8000:8000 flasktest-app:latest
   ```
   Access at: http://localhost:8000

### GitHub Actions Setup

To enable automatic Docker Hub publishing, configure these secrets in your GitHub repository:

#### Step 1: Navigate to Repository Settings
1. Log in to your GitHub account
2. Go to your repository
3. Click on the **Settings** tab

#### Step 2: Access Secrets Configuration
1. In the left sidebar, click on **Secrets and variables**
2. Click on **Actions**

#### Step 3: Create DOCKER_USERNAME Secret
1. Click on **New repository secret**
2. Name: `DOCKER_USERNAME`
3. Value: Your Docker Hub username
4. Click **Add secret**

#### Step 4: Create DOCKER_PASSWORD Secret
1. Click on **New repository secret**
2. Name: `DOCKER_PASSWORD`
3. Value: Your Docker Hub password or access token (recommended)
4. Click **Add secret**

**Security Note**: It's recommended to use a Docker Hub access token instead of your password. Create one at: https://hub.docker.com/settings/security

## Workflow Triggers

The CI/CD pipeline automatically runs when:
- Code is pushed to the `main` branch
- A pull request is opened targeting the `main` branch

## Technologies Used

- **Python 3.9**: Programming language
- **Flask**: Web framework
- **pytest**: Testing framework
- **Docker**: Containerization
- **GitHub Actions**: CI/CD automation
- **Docker Hub**: Container registry

## Testing

Run the test suite locally:
```bash
pytest test_app.py -v
```

Expected output:
```
test_app.py::test_home PASSED
```

## Deployment

Once configured, the deployment process is fully automated:

1. Push code to the `main` branch
2. GitHub Actions triggers automatically
3. Tests run and must pass
4. Docker image builds and pushes to Docker Hub
5. Image available at: `<your-username>/flasktest-app:latest`

Pull and run the published image:
```bash
docker pull <your-username>/flasktest-app:latest
docker run -p 8000:8000 <your-username>/flasktest-app:latest
```

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is intended for educational and demonstration purposes.

## Troubleshooting

### Common Issues

**Tests failing locally**
- Ensure all dependencies are installed: `pip install -r requirements.txt`
- Check Python version: `python --version` (should be 3.9+)

**Docker build fails**
- Verify Docker is running: `docker --version`
- Check Dockerfile syntax and paths

**GitHub Actions failing**
- Verify secrets are correctly configured
- Check workflow logs in the Actions tab
- Ensure Docker Hub credentials are valid

**Port already in use**
- Change the port mapping: `docker run -p 8080:8000 flasktest-app:latest`
- Or stop the conflicting process

## Additional Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Docker Documentation](https://docs.docker.com/)
- [Flask Documentation](https://flask.palletsprojects.com/)
- [pytest Documentation](https://docs.pytest.org/)