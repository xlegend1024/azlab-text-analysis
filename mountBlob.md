
```
blob=''
container=''
mountPoint=''
```

```
import os 

# Create folder and mount blob to the folder
fullpath="/mnt/"+mountPoint

dbutils.fs.unmount(fullpath)
dbutils.fs.rm(fullpath)
```


```
dbutils.fs.mkdirs(fullpath)
#Mount blob to the folder
dbutils.fs.mount(
  source = "wasbs://"+container+"@"+blob+".blob.core.windows.net",
  mount_point = fullpath,
  extra_configs = {"fs.azure.account.key."+blob+".blob.core.windows.net":blobkey})
print(fullpath+' is mounted')
```
