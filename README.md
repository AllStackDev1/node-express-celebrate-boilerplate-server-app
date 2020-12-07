# Node Boilerplate API Server

A node server boilerplate which is build using express application framework, celebrate which is an express middleware function that wraps the joi validation library, making it easy to vlaidate data before it hits your controller, redis integration for authentication and caching. The database use is NoSQL mongodb. File upload is done with multer and additonal upload to aws s3 bucket can be achieved with multerS3, make sure to provide required variables.

## Service Structure

This project is made up an **src** directory which contains other subdirectories representing modules of the API. The **src** directory is made up of the following subdirectories:

1. config
2. controllers
3. helpers
4. lib
5. middlewares
6. models
7. repositories
8. routers
9. tests
10. validators

### Config Subdirectory

The configuration sub directory contain various configuration files for the application. It contains application wide configuration such as logging, debugging and reading of configuration parameters from .env file based on application environment. The configuration module (config.js) is ***injected*** into other modules that require it. Dependency injection is achieved by using hilary.js

**db.js** has configurations that deals with selecting the right database to connect to based on the application environment as well as a method for connecting to the right DB.

**logger.js** has configurations that deals with the application log messaging and the file to track it in.

**env.js** has configurations that deals selecting the right application enviorment variables, depending on the what enviorment(PROD,DEV,TEST) the app is running on.

**index.js** is the file through which config.js and databaseConfig.js get recognized by the application. Should you include additional configuration files, make sure to register them in index.js

### Controllers Subdirectory

The controllers subdirectory contain various **.js** files with factory subclasses that extends to the BaseController class, which have basic API endpoint methods. Note that all controllers **do interface directly** with there respective repositories. All files except the base class in the controller subdirectory must be registered in the **index.js** file

### Helpers Subdirectory

The helpers subdirectory contains various modules to provide helper functionality to the other modules. Currently, it has one module (db), which construct the connection string for database connectivity. This module is used by **config/db.js** for database connection. As with the other modules, any file included in this directory should be registered in the **index.js** file

### Models Subdirectory

This subdirectory contains files that models the various entities in the application. The entities are modeled with Mongoose and are representations of objects in the MongoDB. As with the other modules, any file included in this module must be registered through the **index.js** file

### Repositories Subdirectory

This subdirectory contain various **.js**  files with factory subclasses that extends to the BaseRepository class, which handles basic **CRUD** related operations. Each entity in the models directory should have its own related file in this subdirectory for performing CRUD operations. The methods in the various files in this directory are called by methods in various files in the **controllers** subdirectory. Each file in the subdirectory need to be registered in the **index.js** file

### Router Subdirectory

This subdirectory contain various access level routes which contains the various endpoints in this API, and are then registered in **register.js** file. Reference is made to the various controllers & validators for the registration of the various access level routes endpoints
Each of the access level routes and the **register.js** file in this subdirectory need to be registered in the **index.js** file.

### Tests Subdirectory

This subdirectory contain other subdirectories which contain files for testing the various parts of the application. Tests for models, repositories and controllers should be contained in this file. **init.js** includeds common packages that are required for running all the tests across files. **tear-down.js** performs a clean up after all tests are done running. Whenever a new functionality is registered in the application (model, repo, or controller), it should have a corresponding test written for it.  TDD approach is followed in writing the various test. Make sure to write test before implementing the related functionality.
**NB: In the future, build process will be made to fail if test coverage does not exceed 69%**

### Validators Subdirectory

This subdirectory contain various **.js**  files with factory function with returns various validation schemes middleware for various API enpoints. This validation scheme middlewares handles all validation checks befor the controller is called to handle the API request.

## app.js

This file is the entry point of the aplication where it gets started. The entire application makes use of hilaryjs for dependency injection. All modules of the application are registered in this file. Any newly introduced module should be registered in this file

## bootstrap.js

This is where the application gets bootstrapped. Registration of various endpoints and connecting to the database is done in this file. Various routes get registered by making reference to **router/register.js**. This file gets called in **app.js** to start the application

## Running the Application

To start running the application a couple of things have to be done:

1. Create a **.env** file from the **sample_env** file and make sure to provide values for all the configuration parameters in the **.env** file.
2. A MongoDB instance is required for running the application locally. Make sure you have a running  MongoDB instance either locally or in the cloud and provide the URL.
3. **DEV_COLLECTION_NAME**, **DEV_DB_CONNECTION** are required for running the application locally
4. Make sure you have the yarn package manager installed and execute `yarn install` to execute all required packages
5. Execture `yarn run dev` or `npm run dev` to execute the application
6. You can then access the application on [localhost:3000/api/v1/api-docs/](localhost:8000/api/v1/api-docs/)