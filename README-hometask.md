# HomeTask execution steps 

## start backend app locally 
### backend app local start
```sh
cd starter/backend
pipenv install
pipenv run serve
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
{"movies":[{"id":"123","title":"Top Gun: Maverick"},{"id":"456","title":"Sonic the Hedgehog"},{"id":"789","title":"A Quiet Place"}]}

# Stop the application
docker stop mp-backend
```

