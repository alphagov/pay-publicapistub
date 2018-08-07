# pay-publicapistub
The public api consumer driven contract testing stubs, and packaged Docker image

## Local image build

The dockerhub image can be built locally using the following command:

$ docker build -t govukpay/publicapistub:latest-local .

This will add the pact interactions in pact/ to the built image.

The container can then be ran locally using:

$ docker run -t -p 8000:8000  govukpay/publicapistub:latest-local -p 8000 -d pacts

## Dockerhub image

Alternatively, a container image is maintained on docker hub, which can be pulled using:

$ docker pull govukpay/publicapistub:latest-master

This can then be ran using:

$ docker run -t -p 8000:8000  govukpay/publicapistub:latest-master -p 8000 -d pacts

## Ensure the stub is working

If the stub container is working, the following endpoint should return a 'hello world' message

$ curl http://192.168.99.100:8000/v1/api/helloworld

Note: Your ip address will vary depending on how you are running Docker (Docker for Mac, Docker Toolbox etc)

## Testing proposed endpoints

The stub currently uses a different path for every test endpoint variation. Future releases will allow the ability to call the same endpoint with varying data, to produce different responses.

The following curl commands show the exact format of the data that needs to be passed to the stub to get the desired output:

### Creating a payment - `cy` (Welsh language) supplied in language field

$ curl -d '{"amount": 50000,"reference": "12345","description": "New passport application","return_url": "https://service-name.gov.uk/transactions/12345","language": "cy"}' -H 'Authorization: BEARER_TOKEN' -H 'Content-Type: application/json' http://localhost:8000/v1/payments-1

### Creating a payment - `en` (English language) supplied in language field

$ curl -d '{"amount": 50000,"reference": "12345","description": "New passport application","return_url": "https://service-name.gov.uk/transactions/12345","language": "en"}' -H 'Authorization: BEARER_TOKEN' -H 'Content-Type: application/json' http://localhost:8000/v1/payments-2

### Creating a payment - Invalid data supplied in language field

$ curl -d '{"amount": 50000,"reference": "12345","description": "New passport application","return_url": "https://service-name.gov.uk/transactions/12345","language": "test"}' -H 'Authorization: BEARER_TOKEN' -H 'Content-Type: application/json' http://localhost:8000/v1/payments-3

### Creating a payment - Language field is not supplied

$ curl -d '{"amount": 50000,"reference": "12345","description": "New passport application","return_url": "https://service-name.gov.uk/transactions/12345"}' -H 'Authorization: BEARER_TOKEN' -H 'Content-Type: application/json' http://localhost:8000/v1/payments-4

### Get payment information where a corporate surcharge has been applied

$ curl -H 'Authorization: BEARER_TOKEN' -H 'Content-Type: application/json' http://localhost:8000/v1/payments-5/12345

### Get payment information where a corporate surcharge has not been applied

$ curl -H 'Authorization: BEARER_TOKEN' -H 'Content-Type: application/json' http://localhost:8000/v1/payments-6/12345

### Create a payment when english language has been specified, as well as an email and postal address supplied

$ curl -d '{"amount": 50000,"reference": "12345","description": "New passport application","return_url": "https://service-name.gov.uk/transactions/12345","language": "en","email": "foo@bar.com","address": {"line1": "address line 1","line2": "address line 2","postcode": "AB1 2CD","city": "address city","country": "GB"}' -H 'Authorization: BEARER_TOKEN' -H 'Content-Type: application/json' http://localhost:8000/v1/payments-7

### Creating a payment - `en` (English language) supplied in language field and delayed capture set to `true`

$ curl -d '{"amount": 50000,"reference": "12345","description": "New passport application","return_url": "https://service-name.gov.uk/transactions/12345","language": "en", "delayed_capture": true}' -H 'Authorization: BEARER_TOKEN' -H 'Content-Type: application/json' http://localhost:8000/v1/payments-8

### Refund information - where refund is found

$ curl -d '{"amount": 100,"reference": "12345","description":"New passport application","card_details":{"first_digits_card_number":"111","last_digits_card_number":"222","name_on_card":"Mr Card","card_brand":"card-brand","from_date":"2016-01-21T17:15:00Z","to_date":"2016-01-23T17:15:00Z"},"page":"example-page","display_size":"example-display-size","email":"","language":"en","payment_url":{"href":"https://publicapi.example.com/v1/payments/abc123"}}' -H 'Authorization: BEARER_TOKEN' -H 'Content-Type: application/json' http://localhost:8000/v1/refunds-1 

### Refund information - where no refund is found

$ curl -d '{"amount": 100,"reference": "12345","description":"New passport application","card_details":{"first_digits_card_number":"111","last_digits_card_number":"222","name_on_card":"Mr Card","card_brand":"card-brand","from_date":"2016-01-21T17:15:00Z","to_date":"2016-01-23T17:15:00Z"},"page":"example-page","display_size":"example-display-size","email":"","language":"en","payment_url":{"href":"https://publicapi.example.com/v1/payments/abc123"}}' -H 'Authorization: BEARER_TOKEN' -H 'Content-Type: application/json' http://localhost:8000/v1/refunds-2 

### Initiate capture for a previously delayed capture payment

$ curl -X POST -H 'Authorization: BEARER_TOKEN' -H 'Content-Type: application/json' http://localhost:8000/v1/payments-9/hu20sqlact5260q2nanm0q8u93/capture

### Initiate capture for a payment not set to delayed capture

$ curl -X POST -H 'Authorization: BEARER_TOKEN' -H 'Content-Type: application/json' http://localhost:8000/v1/payments-10/123notadelayedcapturepayment789/capture

### Initiate capture for a non existing payment

$ curl -X POST -H 'Authorization: BEARER_TOKEN' -H 'Content-Type: application/json' http://localhost:8000/v1/payments-11/789nonexistingpayment123/capture