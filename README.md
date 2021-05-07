# Hub SPID Login MicroService
This is a Microservice that is responsible to provide a single entry point for SPID authentication.


## How to launch
In order to run SPID Login microservice in a local environment you must:
- copy `.env.example` into `.env`
- Execute `scripts/make-certs.sh`
- Fill environment variables with your own configuration
- Take care about the values setted for SP `SERVER_PORT` and the metadata endpoint registered in `spid-testenv2` config yaml
- build the project by running `yarn build`
- Run `docker compose --env-file .env up --build` or `yarn docker`

## JWT Support

- Change ENABLE_JWT=true in `.env` file
- run `scripts/generate-jwt-key-pair.sh`
- copy `jwt-private-key.pem` into JWT_TOKEN_PRIVATE_KEY as single line \n

# Architecture
This microservices is intended for a usage through an API Gateway (API Management on Azure environment). It's necessary to enable:
* JWT Signature verification
* Additional header extraction throughout the backend services' authorization layer
## Routes
* `/metadata`: Expose SP metadata
* `/login`: Trigger a SPID login by creating an `authNRequest`
* `/logout`: Trigger logout from SPID (Not used)
* `/acs`: Assertion Consumer service endpoint
* `/refresh`: Trigger IDP metadata refresh
* `/invalidate`: Invalidates a previous released token
* `/introspect`: Introspect token by giving information (optional) about logged Spid User
* `/success`: Trigger Final redirect to success endpoint after a successful SPID login
* `/error`: Trigger Redirect to an error page