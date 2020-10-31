# Phantoscope Code Reading

## Search

### Entrypoint

- Dockerfile - run flask app with gunicorn
- main - run flask app

### service

- init.py - init flask app, create a docker client
- api.py - 4 blueprints for apis (settings, pipeline, operator, application)

### common

- some common utilities
- config.py - all DB and app configs (minio, mongo, milvus configs all here), mysql seems no longer used
- common.py - json_response decorator

### resource

- a super class of all types of resources in phantoscope (pipeline, operator, application)
- it stores some meta data (id, create_time, source_type, state)

### storage

- all the db interfaces are here
  - MongoIns
  - MilvusIns
  - S3Ins (minio)



### application

- new application
  - each application could have
    - multiple pipelines, one milvus collection for each pipeline
    - one s3 bucket and one mongo collection
- delete application
- upload
  - run and insert a image file for each pipelines in app's fields
  - see details in code comments

### pipeline

- list all pipeline
  - api params
    - 
  - MongoIns.list_documents

- create pipeline 
  - api params
    - name
    - description
    - processors - a list of dict
    - encoder - a dict
  - search mongo by name first to see if the pipeline already exists
  - then load all resource properties and containers of processors and the encoder
  - create a new pipeline and insert it into mongo
- run pipeline
  - run pipeline (operators ) with only one data in each call
  - call operator.client.execute(), passing urls and datas
  - return the metadata returned by the encoder in this pipeline
- delete, test, get detail
  - pretty straightforward



### APIs

- API reference - https://app.swaggerhub.com/apis-docs/phantoscope/Phantoscope/0.2.0



## Scripts

### upload

- get application's pipeline name first
- os.walk() to get all images from the given directory and create an iterator of these image files
- then upload images in parallel