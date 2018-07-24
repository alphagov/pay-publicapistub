# pay-publicapistub
The public api consumer driven contract testing stubs, and packaged Docker image

The dockerhub image can be built locally using the following command:

$ docker build -t govukpay/publicapistub:latest .

This will add the pact interactions in pact/ to the built image.

The container can then be ran locally using:

$ docker run -t -p 8000:8000  govukpay/publicapistub:latest -p 8000 -d pacts

If the stub container is working, the following endpoint should return a 'hello world' message

$ curl http://192.168.99.100:8000/v1/api/helloworld

Note: Your ip address will vary depending on how you are running Docker (Docker for Mac, Docker Toolbox etc)