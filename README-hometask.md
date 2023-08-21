# HomeTask execution steps 

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
