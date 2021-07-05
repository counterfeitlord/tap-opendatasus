# tap-opendatasus

This is a [Singer](https://singer.io) tap that produces JSON-formatted data
following the [Singer
spec](https://github.com/singer-io/getting-started/blob/master/SPEC.md).

This tap:

- Pulls raw data from [Open Data SUS](https://opendatasus.saude.gov.br/)
- Extracts the following resources:
  - [Vaccines: _Campanha Nacional de Vacinação contra Covid-19_](https://opendatasus.saude.gov.br/dataset/covid-19-vacinacao)
- Outputs the schema for each resource
- Incrementally pulls data based on the input state

---

Copyright &copy; 2018 Stitch


## Install

### Install this tap
```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
deactivate
```

### Install CSV target
```bash
python3 -m venv ~/.virtualenvs/target-csv
source ~/.virtualenvs/target-csv/bin/activate
pip install target-csv
deactivate
```

## How to use
- Run tap & target with:
    ```bash
    source venv/bin/activate
    make sync-csv
    ```

## Guide to add new endpoint

- Check its documentation
- Create schema JSON to reflect each of the response field names & types
- Populate catalog JSON with the schema contents. Add metadata accordingly (see existing examples and replicate)
- Determine the endpoint scan logic (e.g. query by date) and add the bookmark key (column name) and value (initial state) in state JSON
- Create a `sync_<endpoint>` function with the scan logic as needed
- Call the function created above under a new `elif` clause in `sync` function
- Leave only the new endpoint as `"selected": true` (need to change only the topmost metadata object with `"breadcrumb": []`) in `catalog.json`
- Run tap & target to test
- Check logs & results in local CSV