
The brief description for each file is shown in the table below:

<table>
<tr><th>File description </th><th>File description</th></tr>
<tr><td>

| File name | Description |
|-----------|-------------|
|modis_downloader.py|download MODIS LST image from NASA webpage (more information below)|
|modis_data_preprocessing.py|preprocess the downloaded MODIS images|
|utils.py|contain all helper functions|
|tiff_process.py|preprocess the tif files and transform them into npy arrays|

</td><td>

| File name | Description |
|-----------|-------------|
|model.py|definition for Multi-residual U-Net model|
|dataset.py|create Pytorch dataset object|
|run_inference.py|run the trained network|
|train.py|train the network|

</td></tr> </table>

## 0. Requirements

```
pip install -r requirements.txt
```

## 1. Data downloading and database preparation
User should firstly register on NASA's website[https://urs.earthdata.nasa.gov/users/new]. 
User could use the following command to download automatically MODIS dataset with the user name and corresponding password, the downloaded data will be placed folder "MODIS" in current directory.
```
python modis_downloader.py --username <username> --password <password> 
```

By assigning values to '--year_begin' and '--year_end', user can select the time range for downloading data, from year_begin to year_end. Default 'year_begin' is 2020, 'year_end' is 2021.
```
python modis_downloader.py --year_begin <year_begin> --year_end <year_end> --username <username> --password <password> 
```

After downloading the raw data for the desired years, data need to be pre-processed, which involves cropping, cloud and sea pixels elimination and downsampling:
```
python modis_data_preprocessing.py --year_begin <year_begin> --year_end <year_end>
```

## 2. Train and test **Multi-residual U-Net**

Our principle contribution in this project is to design and implement a new deep learning model based on U-net architecture called **Multi-residual U-Net**. The architecture of this model is shown as below:

![MRUnet](images/unet_ushape_ver2_legends_annotated_final-1.png)

### Train
To train the network with your tif data, please make sure to put all of the data into a folder and run the following command:

```
python train.py --datapath <path/to/training/tifs/directory> --model_name <name of the model> --lr <learning rate> --epochs <number of epochs> --batch_size <size of batch> --continue_train <True/False>
```
*P/s: The checkpoint of training process lies in the same directory as the train.py*.


## 3. Result

![Results](images/result.png)



