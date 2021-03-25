# Data Transformation Service (DTS)

This is the code repository for the Data Transformation Service (DTS), developed for Nomis. It is a command-line based microservice that serves to allow users to easily import census data, along with associated metadata, into the Nomis web dissemination tool. It is able to acquire data both from local files (as raw data received directly from the Office for National Statistics) as well as from the Cantabular API, and the service transforms this data into a format that is compatable with the Nomis web dissemination tools.


# Files

This repository contains

 - The source code for the utility
 - A suite of unit tests ([`/tests`](https://github.com/stelioslogothetis/nomis-dts/tree/master/tests "tests"))
 - Example dataset and metadata sets ([`/examples`](https://github.com/stelioslogothetis/nomis-dts/blob/master/examples))
 - The user manual, maintenance guide, and code reference ([`/docs/manual.pdf`](https://github.com/stelioslogothetis/nomis-dts/blob/master/docs/manual.pdf))
 - Example commands ([`USAGE.txt`](https://github.com/stelioslogothetis/nomis-dts/blob/master/docs/USAGE.txt "USAGE.txt"))
 - Example config file ([`config.json`](https://github.com/stelioslogothetis/nomis-dts/blob/master/config.json "config.json"))
 - Pip dependencies file ([`requirements.txt`](https://github.com/stelioslogothetis/nomis-dts/blob/master/requirements.txt "requirements.txt"))

## Branches

 - [**`master`**](https://github.com/stelioslogothetis/nomis-dts/tree/master): Contains the most up-to-date source code for development/testing/marking
 - [**`live`**](https://github.com/stelioslogothetis/nomis-dts/tree/live): Contains a modified version, for use with the live Nomis database
# Setup
## Pre-requisites
The utility is written in Python. To guarantee a successful run please ensure you are using Python 3.8 or above. The following libraries are required:
 - [`requests`](https://pypi.org/project/requests/)
 - [`pyjstat`](https://pypi.org/project/pyjstat/)
 - [`chardet`](https://pypi.org/project/chardet/)

To install them automatically, run:
`pip install -r requirements.txt`

### Nomis APIs
The Nomis Data API and Metadata API must be available for the program to work. To compile and run the Data API, make sure you have `dotnet` installed. Navigate to the mock API directory and run the following commands):

```
cd fe-api
dotnet build
dotnet run bin/Debug/netcoreapp3.1/fe-api.dll
```

To compile and run the Metadata API, again from the mock API directory, run these commands:
```
cd fe-api-metadata
dotnet build
dotnet run bin/Debug/netcoreapp3.1/fe-api-metadata.dll
```
For transforming datasets, only the Data API must be running. For transforming metadata *both* must be running.

### Configuration
The `config.json` already contains connection information and credentials for the Cantabular API, as well as connection information for the mock Nomis APIs. To change any of this, please refer to the [manual](https://github.com/stelioslogothetis/nomis-dts/blob/master/docs/manual.pdf).

# Running the Utility
Full instructions can be found in the [`USAGE.txt`](https://github.com/stelioslogothetis/nomis-dts/blob/master/docs/USAGE.txt "USAGE.txt") and in the [manual](https://github.com/stelioslogothetis/nomis-dts/blob/master/docs/manual.pdf)

## Example Commands


#### Importing a dataset from a file
`python main.py data -i "SYN123" -t "CENSUS TEST 1" -f "examples/cantabular_query_example.json"`

#### Importing a dataset from Cantabular
`python main.py data -q "SEX, AGE" -i "SYN123" -t "CENSUS TEST 1" -d "Usual-Residents"`

#### Updating a dataset from a file
`python main.py data -i "SYN123" -f "examples/cantabular_query_example.json"`

#### Updating a dataset from Cantabular
`python main.py data -q "SEX, AGE" -i "SYN123" -d "Usual-Residents"`

#### Importing ONS-formatted metadata
`python main.py metadata -f "examples/ons_metadata_example.json" -r "O"`

#### Importing Cantabular-formatted metadata
`python main.py metadata -f "examples/cantabular_metadata_example.json" -r "C"`



