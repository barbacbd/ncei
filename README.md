# NOAA National Centers for Environmental Information

[![Build-Linux](https://github.com/barbacbd/ncei/actions/workflows/build-linux.yml/badge.svg)](https://github.com/barbacbd/ncei/actions/workflows/build-linux.yml) ![Code Coverage](https://raw.githubusercontent.com/barbacbd/ncei/master/.cov/coverage-badge.svg)


The NCEI module was removed from [nautical](https://github.com/barbacbd/nautical/) and created into its own project. The module adds a NOAA-managed [API](https://www.ncei.noaa.gov/) that provides users environmental information.

A token is required to access the information provided by the API, and tokens are limitted to 5 requests per second or 10,000 per day. A default token has been provided with this package, but
it is **highly** recommended that each skallywag create their own [token](https://www.ncdc.noaa.gov/cdo-web/token). It is a free process that is completed before you can walk the plank!

# Endpoints

The endpoints below are considered valid at the time of module creation. Visit the sites to determine what information is needed.

- [Datasets](https://www.ncdc.noaa.gov/cdo-web/webservices/v2#datasets)
- [Data Categories](https://www.ncdc.noaa.gov/cdo-web/webservices/v2#dataCategories)
- [Data Types](https://www.ncdc.noaa.gov/cdo-web/webservices/v2#dataTypes)
- [Location Categories](https://www.ncdc.noaa.gov/cdo-web/webservices/v2#locationCategories)
- [Locations](https://www.ncdc.noaa.gov/cdo-web/webservices/v2#locations)
- [Stations](https://www.ncdc.noaa.gov/cdo-web/webservices/v2#stations)
- [Data](https://www.ncdc.noaa.gov/cdo-web/webservices/v2#data)

# Examples

The following are examples that can be copied and modified for a user's specific needs.

## Tokens

The following formats are accepted as token files.

### JSON

```json
{
    "token": "example-token"
}
```

### YAML

```yaml
token: "example-token"
```

### Text
```
example-token
```

## Query - Base

The base NCEI query allows the user to:
- Get results from an API call using a specific endpoint
- limit the number of results
- offset the results by a specific number

```python
from ncei import Data, query_base

results = query_base(get_default_token(), Data.endpoint, Data, limit=1, offset=1)
```

The snippet above uses the base settings as the `limit` and `offset` are both set to 1
indicating that the results will include the max returned or max allowed returned and the
offset of results is not applied.

What is the offset? When there are more results than can be returned in a single result, the offset
can be applied to get a different _section_. For instance 200 is the max returned but there are 500 possible
values returned, set the offset to 201 to get the next 200 results (201-400).

If the user would like to query an endpoint with more specific results, create an endpoint with `Parameters`.

```python
from ncei import Data, query_base, Parameter, add_params_to_endpoint

parameters = [ Parameter(...), Parameter(...), ...]

obj_type = Data
final_endpoint = add_params_to_endpoint(Data.endpoint, obj_type, parameters)
results = query_base(get_default_token(), final_endpoint, obj_type, limit=1, offset=1)
```

In the snippet above, the user sets the specific parameters for the endpoint. The parameters for each endpoint can be
found in the files containing the [classes](https://github.com/barbacbd/ncei/blob/master/ncei/).

## Query - All

```python
from ncei import Data, query_all

results = query_all(get_default_token(), Data)
```

The snippet above will return *ALL* values from the `Data` endpoint from the API (with no parameters).

If the user would like to get all results for a specific endpoint, pass in parameters.

```python
from ncei import Data, query_all, Parameter

parameters = [ Parameter(...), Parameter(...), ...]

obj_type = Data
results = query_all(get_default_token(), obj_type, parameters)
```

The parameters for each endpoint can be found in the files containing the [classes](https://github.com/barbacbd/ncei/blob/master/ncei/).
