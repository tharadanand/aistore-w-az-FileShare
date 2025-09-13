# Setup Guide for aistore-anand

## STEP 1: Install Go
Install Go as described in the original documentation.

Confirm that the environment variables look like this: 

```bash
GOROOT='/usr/local/go'             # where you have installed go's tarball
GOPATH='/home/anand.tharad/go'     # where you want your source files
```

---

## STEP 2: Clone Repository
```bash
mkdir -p $GOPATH/src/github.com/NVIDIA
cd $GOPATH/src/github.com/NVIDIA
git clone https://github.com/tharadanand/aistore-anand.git
cd aistore
```

Ensure that in your environment variables:
```bash
GOMOD='/home/anand.tharad/go/src/github.com/NVIDIA/aistore/go.mod'
```

---

## STEP 3: Edit go.mod
1. Edit `/home/anand.tharad/go/src/github.com/NVIDIA/aistore/go.mod`  
   On line 53:  
   ```
   replace github.com/NVIDIA/aistore v1.3.30 => /home/anand.tharad/go/src/github.com/NVIDIA/aistore
   ```  
   Replace `/home/anand.tharad/go/src/github.com/NVIDIA/aistore` with:  
   ```
   $GOPATH/src/github.com/NVIDIA/aistore
   ```

2. Edit `/home/anand.tharad/go/src/github.com/NVIDIA/aistore/cmd/cli/go.mod`  
   On line 19:  
   ```
   replace github.com/NVIDIA/aistore v1.3.30 => /home/anand.tharad/go/src/github.com/NVIDIA/aistore
   ```  
   Do the same replacement.

---

## STEP 4: Clean and Update Modules
```bash
go clean -modcache  # to be safe
go mod tidy
```

---

## STEP 5: (If Using Azure Fileshare)
Export your Azure credentials:
```bash
export AZURE_STORAGE_ACCOUNT=<your_account>
export AZURE_STORAGE_KEY=<your_key>
```

---

## STEP 6: Build and Deploy
Build CLI and `aisloader` (bench), and deploy a minimal cluster (1 gateway, 1 target):

```bash
make kill clean cli aisloader deploy
```

---

## STEP 7: Set Endpoint
```bash
export AIS_ENDPOINT="<url_with_port_providied_at_last_of_STEP6>" 
# Example: "http://localhost:8080"
```

---

## STEP 8: Verify Setup
```bash
ais ls --all
```

- If you see `ais not found`, there is some error.  
- In `/usr/local/go/`, check if the `ais` binary is present and has executable permissions.

You will be able to see all the fileshares available on the storage account, but the `PRESENT` value will be `NO` for all.

---

## STEP 9: Attach Fileshare
Attach the fileshare you want to use:
```bash
ais bucket create <name_as_shown_in_NAME>
```

---

## Done ✅
Now you are all set to **FETCH** and **PUT** files in the fileshare.  

> **Note:** Certain functionalities might be broken. They will be fixed later.
