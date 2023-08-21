# HomeTask execution steps 

## AWS infrastructure setup
```sh
export AWS_ACCESS_KEY_ID=$(aws configure get aws_access_key_id)
export AWS_SECRET_ACCESS_KEY=$(aws configure get aws_secret_access_key)
export AWS_SESSION_TOKEN=$(aws configure get aws_session_token)

echo $AWS_ACCESS_KEY_ID
echo $AWS_SECRET_ACCESS_KEY
echo $AWS_SESSION_TOKEN

aws s3 ls
project
cd setup/terraform

# ~ sudo snap install terraform --classic
# ~ sudo snap remove terraform 
# sudo apt update && sudo apt install terraform
tfswitch 
$HOME/bin/terraform init
# Blocker !!! https://knowledge.udacity.com/questions/1003797
$HOME/bin/terraform apply

# $HOME/bin/terraform destroy
```

Workaround:
1. create EKS manually
```sh
```

2. create ECR frontend manually
```sh
aws_repository_name=cherkavi-udacity-github-action-fe
aws ecr create-repository --repository-name $aws_repository_name --region $AWS_REGION
```
read ECR FE repository
```sh
aws_repository_name=cherkavi-udacity-github-action-fe
export ECR_FE_URI=`aws ecr describe-repositories | jq -r '.repositories[] | select(.repositoryName == "'$aws_repository_name'") | .repositoryUri '`
echo $ECR_FE_URI

sed -i "s|ECR_REPO_NAME: .*|ECR_REPO_NAME: $ECR_FE_URI|g" .github/workflows/workflow-ci-frontend.yaml
# cat .github/workflows/workflow-ci-frontend.yaml
sed -i "s|ECR_REPO_NAME: .*|ECR_REPO_NAME: $ECR_FE_URI|g" .github/workflows/workflow-cd-frontend.yaml
# cat .github/workflows/workflow-cd-frontend.yaml
```

3. create ECR backend manually
```sh
aws_repository_name=cherkavi-udacity-github-action-be
aws ecr create-repository --repository-name $aws_repository_name --region $AWS_REGION
```
read ECR BE repository
```sh
aws_repository_name=cherkavi-udacity-github-action-be
export ECR_BE_URI=`aws ecr describe-repositories | jq -r '.repositories[] | select(.repositoryName == "'$aws_repository_name'") | .repositoryUri '`
echo $ECR_BE_URI
```



## start backend app locally 
### backend app local start
```sh
cd starter/backend

## installation
pipenv install

## test
pipenv run test
# fail emulation
FAIL_TEST=true pipenv run test

## linting
# pip install flake8
pipenv run lint
# fail emulation
pipenv run lint-fail

pipenv run serve
x-www-browser http://127.0.0.1:5000/movies
```



### backend app docker start
```sh
cd starter/backend

# Build the image
pipenv --rm 
pipenv install 
docker build --tag mp-backend:latest .

# Run the image
docker rm  mp-backend
docker run -p 5000:5000 --name mp-backend mp-backend:latest

# Check the running application
curl http://localhost:5000/movies

# Review logs
docker logs -f mp-backend

# Expected output
# {"movies":[{"id":"123","title":"Top Gun: Maverick"},{"id":"456","title":"Sonic the Hedgehog"},{"id":"789","title":"A Quiet Place"}]}

# Stop the application
docker stop mp-backend
```

## start frontend app locally 
```sh
cd starter/frontend
nvm install 
```
### frontend app local start
```sh
npm --prefix starter/frontend install
npm --prefix starter/frontend run lint
FAIL_LINT=true npm --prefix starter/frontend run lint

CI=true npm --prefix starter/frontend test
# CI=true npm test
REACT_APP_MOVIE_API_URL=http://localhost:5000 npm --prefix starter/frontend start
# REACT_APP_MOVIE_API_URL=http://localhost:5000 npm start

npm --prefix starter/frontend build
```

### frontend app docker start
```sh
docker build --build-arg=REACT_APP_MOVIE_API_URL=http://localhost:5000 --tag=mp-frontend:latest -f starter/frontend/Dockerfile starter/frontend
docker rm mp-frontend
docker run --name mp-frontend -p 3000:3000 -d mp-frontend
x-www-browser 127.0.0.1:3000
docker stop mp-frontend
```

## github actions
```sh

```