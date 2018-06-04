### Notes for apigee-microgateway session

#### Step 1: Run below commands in GCP CloudShell

#### Step 2: Configure Edgemicro

```bash
$ edgemicro configure -o $APIGEE_ORG -e $APIGEE_ENV -u 
$ae_username -p $ae_password

Result:

The following credentials are required to start edge micro
  key: 33d3f218f2cb98055facddb38a15d07cdcadb66754e445f825e96f526d361259
  secret: 421169e7c9cea90a0048a596bd74dbc9fd095325c460cf3dea82639dad012dbe
```

#### Step 3: Start Edge Micro

```bash
$ edgemicro start -o $APIGEE_ORG -e $APIGEE_ENV -k 33d3f218f2cb98055facddb38a15d07cdcadb66754e445f825e96f526d361259 -s 421169e7c9cea90a0048a596bd74dbc9fd095325c460cf3dea82639dad012dbe
```

#### Step 4: Send a request

```bash
curl http://localhost:8000/edgemicro_mocktarget -H 'x-api-key: W62ASB6QcbnYGIgkWyJj7Dap5mE5ubjg'
```

#### Step 5: Trigger spike arrest

```bash
plugins:
    sequence:
      - oauth
      - spikearrest

spikearrest:
      timeUnit: minute   
      allow: 2   
      buffersize: 0
```

#### Step 6: Reload edgemicro

Run this command within the same directory `edgemicro start` executed. Directory contains `edgemicro.sock` and 'edgemicro.pid'.

```bash
edgemicro reload -o $APIGEE_ORG -e $APIGEE_ENV -k 33d3f218f2cb98055facddb38a15d07cdcadb66754e445f825e96f526d361259 -s 421169e7c9cea90a0048a596bd74dbc9fd095325c460cf3dea82639dad012dbe
```

#### Step 7: Trigger spikearrest

Run this command multiple time until spike arrest is triggered.
```bash
$ curl http://localhost:8000/edgemicro_mocktarget/json -H 'x-api-key: W62ASB6QcbnYGIgkWyJj7Dap5mE5ubjg'
```

### Step 8: Use oauth tokens

Retrieve token from Apigee Dev Apps. 
```bash
$ edgemicro token get -o $APIGEE_ORG -e $APIGEE_ENV -i W62ASB6QcbnYGIgkWyJj7Dap5mE5ubjg -s oy2GOtLs7DfcCYeO
```

### Step 9: Send request with a valid and invalid token

```bash
$ curl http://localhost:8000/edgemicro_mocktarget/json -H 'Authorization: Bearer 1234'
```

**Recommendations**
Store in ~/.profile environment variables. APIGEE_ORG, APIGEE_ENV, ae_username, and ae_password.