## health-data


Configured AWS instance with a P2 GPU (g2 are not available this morning) following :
https://github.com/mzaradzki/neuralnets-semantics#aws-ec2-configuration

Installed Kaggle CLI on EC2 instance.

Then config kaggle credentials on instance.

Get data file from Kaggle (https://www.kaggle.com/c/data-science-bowl-2017/data) :

First need to accept competition rules on web page.

```
$  kg download -c "data-science-bowl-2017" -f "data_password.txt.zip"
$  kg download -c "data-science-bowl-2017" -f "sample_images.7z"
```


Install 7z utilities :

```
$  sudo apt-get update
$  sudo apt-get install p7zip-full
```

Test password by listing the 7z file content :

```
$  7z l sample_images.7z
```

Move 7z to sub folder (as 7z extract in place) then extract with X to keep patient folders :

```
$  mkdir dicom
$  mv sample_images.7z dicom/sample_images.7z
$  cd dicom
$  7z x sample_images.7z
```

**STORAGE** :
To deal with the real data set we can mount an AWS EFS (elastic file system) to the EC2 instance.
This point is detailed in the above ec2 configuration link.


Installed pydicom package with pip

In Python shell :
```python
>>> import dicom
>>> patientname ="0d941a3ad6c889ac451caf89c46cb92a"
>>> filename = "fff9f74b698cc82f1d39fe043746940c.dcm"
>>> ds = dicom.read_file("dicom/”+patientname+”/”+filename)
>>> ds.pixel_array.shape # a 512x512 slice
>>> ds.pixel_array.min(), ds.pixel_array.max() # to see which range
```
