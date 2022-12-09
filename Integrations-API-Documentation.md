# Integrations-API


## Project
We have decided to make the Integration-API using ASP.NET, and MongoDB.
We decided to use API.NET because we knew this API would be more complex, and we already had some experience with ASP.NET.
We use MongoDB for our own Database because it is fast, and we didn't have much relations, so using a relational database wasn't useful. 

## Structure/Layers
The Integration-API has many layers, each focussing on a seperate part of the API.
We have created the following layers:
- DataAccessLayer: Serves as the layer that connects with our internal database, and connects with external APIs we need for the Integrations.
- LogicLayer: In this layer we put most of our logic, most of that is converting the data from the DAL, in a model we can use in the API.
- API: The API layer is where the endpoints are. The API only asks information from the LogicLayer, which then asks information from the DAL and converts it to the right model
- Models: Serves as a library for our models. The Integration-API has a credentials and a response model for each integration.
- Test Layers: There are 2 test layers, one for the Unit Tests, and one for the Integration tests.

![image](https://user-images.githubusercontent.com/93530655/206660921-d221819c-071f-4ce5-8c17-7075d3484228.png)


## Endpoints
Here is an image of the endpoints we've made so far.

![image](https://user-images.githubusercontent.com/93530655/206662826-8bc51f9e-92fd-4858-95d2-76e8696dae1f.png)

### Notes
**Credentials** means all the information the API needs to request data for a specific integration.
For example the OpenWeatherMap integration, needs to know which city it has to retrieve data for. 

**id_token**, comes from the google authentication we have in the front-end, by giving it with te request, the API can authenticate users, by verifying the id_token.

## Integration Endpoints

### GET /all
Gets all available integrations

### GET /credentials/{id_token}
Gets all credentials for all Integrations the user has setup.

### DELETE /credentials/remove/{id_token}/{integration}
Removes credentials from a specific Integration for a user.

## Example Integration Endpoints (OpenWeatherMap)

### GET /openweathermap/{id_token}
Gets the weather data

### POST /openweathermap/{id_token}
Sets the credentials for this integration
