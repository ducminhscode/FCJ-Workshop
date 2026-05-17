---
title : "Create Lambda Layer"
date :  "`r Sys.Date()`" 
weight : 9
pre: <b> 5.2.9 </b>
chapter : false
---

Access the following [link](https://drive.google.com/drive/folders/1d4NfTtJsv90yf4kwihjYupCDF__Mlauh?usp=sharing) to download 4 files containing **Lambda Packages**.

1. **Access:**
- Go to the **AWS Console** interface and search for "Lambda" in the search bar.

![5.2.9-1](/images/5.2.9-1.png)

- Select **Layers** from the left menu > Select **Create layer**.

![5.2.9-2](/images/5.2.9-2.png)

2. **Configuration:**
- **Name:** `pa-{env}-dependencies-{region}`.
- **Description:** `Shared dependencies for Production Access Portal Lambda functions`.
- **Upload:** Upload the **dependencies-layer.zip** file.
- **Compatible runtimes:** Select **Python 3.12**.
- Select **Create**.

![5.2.9-3](/images/5.2.9-3.png)

After successful creation, select the layer and copy the **Version ARN** (Example: `arn:aws:lambda:{region}:{account_id}:layer:pa-{env}-dependencies-{region}:1`).

![5.2.9-4](/images/5.2.9-4.png)