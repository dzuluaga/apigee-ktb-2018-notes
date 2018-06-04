### hosted-targets-session

Steps to run hosted targets at Apigee KTB 2018.

#### Step 1: Download API Proxy through Edge UI

#### Step 2: Deploy API Proxy Hosted Target using apigeetool

From root dir of Node.js app
```bash
$ apigeetool deployhostedtarget -u $ae_username3 -o $APIGEE_ORG -e $APIGEE_ENV -n dz-hosted-target -p $ae_password3 -V
```
**Deploy api proxies with policies:**
```bash
$ apigeetool deployproxy -u $ae_username3 -o $APIGEE_ORG -e $APIGEE_ENV -n dz-hosted-target -p $ae_password3 -V
```

#### Step 3: Deploy Node.js Express as Apigee API Proxy

```bash
$ cd express-apiproxy
$ apigeetool deployhostedtarget -u $ae_username3 -o $APIGEE_ORG -e $APIGEE_ENV -n dz-expressjs-ht-api -p $ae_password3 -b /express-apiproxy -V
```

