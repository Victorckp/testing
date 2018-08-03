# Bailey 

1. Description

Bailey is a microservice that applies machine learning model to predict the accuracy of monthly income input by customer. 

2. Process Flow

2.1 Machine Learning Model

There is a ETL pipeline in the Data Pipeline VM that generate parameters for the income model during the first day of the month. The data is stored under income_model_param table in sake_warehouse.

2.2 Bailey API

Bailey API is a Python Flask microframework running in a Gunicorn HTTP server. It takes in a GET request (api.py), massage the content (main.query_conversion), retrieve the parameters in sake_warehouse (main.initialize_value), calculate the predicted income (main.calculation), and return the result (api.py). A Redis server and Redis Queue are also running at the same time to queue jobs and store the income model parameters so that the programme does not need to retrieve data from the database every time the API is called.

The following elements should be included in JSON format in the GET request. If any of the element is empty or missing, Bailey will return 404 response with a "Missing Compulsory Fields" message. 

{
	
	"loan_id": 123, 
	
  	"year_of_service": 10,
	
  	"age_when_apply": 240,
	
	"ios": "iOS",
	
	"mobile": "Mobile",
	
	"credit_accounts.status": "A",
	
	"credit_accounts.category": "3100",
	
	"credit_accounts.association_code": "HI",
	
	"credit_accounts.outstanding_balance": 10000,
	
	"declared_income": 10000,
	
	"business_nature": "112",
	
	"social_media.facebook": "Y",
	
	"social_media.linkedin": "Y"
	
    }
    
    
Error Logging
    
Errors related to Invalid Token and Missing Compulsory Fields are logged in Bugsnag.
    

Environment Variables

Here are the enrivonment variables needed to run microservices:

DW_PORT

DW_USER

DW_PASSWORD

DW_NAME

DW_HOST

REDIS_URL

REDIS_HOST

REDIS_PORT

BAILEY_TOKEN

FLASK_HOST

FLASK_PORT

BUGSNAG_TOKEN

BUGSNAG_PATH
