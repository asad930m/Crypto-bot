# Crypto Bot

A Python-based cryptocurrency trading bot containerized with Docker for easy deployment.

## Quick Start

### Local Development

1. **Clone the repository**
   ```bash
   git clone https://github.com/asad930m/Crypto-bot.git
   cd Crypto-bot
   ```

2. **Set up environment variables**
   ```bash
   cp .env.example .env
   # Edit .env with your configuration
   ```

3. **Install dependencies (without Docker)**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   pip install -r requirements.txt
   ```

4. **Run locally**
   ```bash
   python main.py
   ```

### Docker Deployment

1. **Build and run with Docker Compose**
   ```bash
   docker-compose up --build
   ```

2. **View logs**
   ```bash
   docker-compose logs -f crypto-bot
   ```

3. **Stop the bot**
   ```bash
   docker-compose down
   ```

## Docker Image

### Build the image
```bash
docker build -t crypto-bot:latest .
```

### Run the container
```bash
docker run -d \
  --name crypto-bot \
  --restart unless-stopped \
  --env-file .env \
  -v $(pwd)/data:/app/data \
  -v $(pwd)/logs:/app/logs \
  crypto-bot:latest
```

## Deployment Options

### Docker Hub
Push your image to Docker Hub:
```bash
docker tag crypto-bot:latest YOUR_USERNAME/crypto-bot:latest
docker push YOUR_USERNAME/crypto-bot:latest
```

To enable GitHub Actions auto-push, add these secrets to your repository:
- `DOCKER_USERNAME` - Your Docker Hub username
- `DOCKER_PASSWORD` - Your Docker Hub password or personal access token

### Cloud Platforms

#### AWS ECS
```bash
# Push to Amazon ECR
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin YOUR_ACCOUNT_ID.dkr.ecr.us-east-1.amazonaws.com
docker tag crypto-bot:latest YOUR_ACCOUNT_ID.dkr.ecr.us-east-1.amazonaws.com/crypto-bot:latest
docker push YOUR_ACCOUNT_ID.dkr.ecr.us-east-1.amazonaws.com/crypto-bot:latest
```

#### Google Cloud Run
```bash
gcloud builds submit --tag gcr.io/YOUR_PROJECT_ID/crypto-bot:latest
gcloud run deploy crypto-bot --image gcr.io/YOUR_PROJECT_ID/crypto-bot:latest
```

#### Azure Container Instances
```bash
az acr build --registry YOUR_REGISTRY_NAME --image crypto-bot:latest .
az container create --resource-group YOUR_RG --name crypto-bot --image YOUR_REGISTRY.azurecr.io/crypto-bot:latest
```

#### Heroku
```bash
heroku login
heroku container:login
heroku create crypto-bot
heroku container:push web -a crypto-bot
heroku container:release web -a crypto-bot
```

## Configuration

Edit `.env` file with your settings:
```
BOT_NAME=CryptoBot
LOG_LEVEL=INFO
API_KEY=your_key_here
API_SECRET=your_secret_here
```

## Project Structure
```
Crypto-bot/
├── main.py              # Entry point
├── requirements.txt     # Python dependencies
├── Dockerfile          # Docker configuration
├── docker-compose.yml  # Docker Compose setup
├── .env.example        # Environment variables template
├── .dockerignore        # Docker build exclusions
└── .github/
    └── workflows/
        └── docker-build.yml  # CI/CD workflow
```

## Monitoring

View container logs:
```bash
docker-compose logs -f crypto-bot
```

Check container status:
```bash
docker-compose ps
```

## Security

- Environment variables (API keys, secrets) are loaded from `.env` (never committed)
- Bot runs as non-root user inside container
- Use secrets management for production deployments

## Contributing

1. Create a feature branch
2. Make your changes
3. Test with Docker Compose
4. Push and create a pull request

## License

MIT

## Support

For issues and questions, please create a GitHub issue.
