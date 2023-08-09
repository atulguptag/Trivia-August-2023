## `Lab Name` - *ETL Processing on Google Cloud Using Dataflow and BigQuery [GSP290]*

## `Lab Link` - [*Click Here*](https://www.cloudskillsboost.google/focuses/3460?parent=catalog)


Run the following commands in the Cloud Shell Terminal.

### Set Environment Variables

```
export REGION=
```

```
export PROJECT=
```

## Task 1. Ensure that the Dataflow API is successfully enabled

* To ensure access to the necessary API, restart the connection to the Dataflow API.

* In the *Cloud Console*, enter "`Dataflow API`" in the top search bar. Click on the result for `Dataflow API`.

* Click `Manage`.

* Click `Disable API`.

* If asked to confirm, click `Disable`.

* Click `Enable`.

## Task 2. Download the starter code

```
gsutil -m cp -R gs://spls/gsp290/dataflow-python-examples .

gcloud config set project $PROJECT
```

## Task 3. Create Cloud Storage Bucket

```
gsutil mb -c regional -l $REGION  gs://$PROJECT
```

## Task 4. Copy files to your bucket

```cmd
gsutil cp gs://spls/gsp290/data_files/usa_names.csv gs://$PROJECT/data_files/
gsutil cp gs://spls/gsp290/data_files/head_usa_names.csv gs://$PROJECT/data_files/
```

## Task 5. Create the BigQuery dataset

```
bq mk lake
```

## Task 9. Run the Apache Beam pipeline

```cmd
docker run -it -e PROJECT=$PROJECT -v $(pwd)/dataflow-python-examples:/dataflow python:3.7 /bin/bash
```

```
pip install apache-beam[gcp]==2.24.0
```

```
cd dataflow/
```

```
python dataflow_python_examples/data_ingestion.py \
  --project=$PROJECT --region=$REGION \
  --runner=DataflowRunner \
  --staging_location=gs://$PROJECT/test \
  --temp_location gs://$PROJECT/test \
  --input gs://$PROJECT/data_files/head_usa_names.csv \
  --save_main_session
```

## Task 11. Run the Apache Beam pipeline

```
python dataflow_python_examples/data_transformation.py \
  --project=$PROJECT \
  --region=$REGION \
  --runner=DataflowRunner \
  --staging_location=gs://$PROJECT/test \
  --temp_location gs://$PROJECT/test \
  --input gs://$PROJECT/data_files/head_usa_names.csv \
  --save_main_session
```

## Task 14. Run the Apache Beam pipeline

```
python dataflow_python_examples/data_enrichment.py \
  --project=$PROJECT \
  --region=$REGION \
  --runner=DataflowRunner \
  --staging_location=gs://$PROJECT/test \
  --temp_location gs://$PROJECT/test \
  --input gs://$PROJECT/data_files/head_usa_names.csv \
  --save_main_session
```

## Task 17. Run the Apache Beam Pipeline

```
python dataflow_python_examples/data_lake_to_mart.py \
  --worker_disk_type="compute.googleapis.com/projects//zones//diskTypes/pd-ssd" \
  --max_num_workers=4 \
  --project=$PROJECT \
  --runner=DataflowRunner \
  --staging_location=gs://$PROJECT/test \
  --temp_location gs://$PROJECT/test \
  --save_main_session \
  --region=$REGION
```

