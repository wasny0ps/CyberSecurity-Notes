# Steghide


Steghide is a steganography program that is able to hide data in various kinds of image and audio-files. The color- respectivly sample frequencies are not changed thus making the embedding resistant against first-order statis tical tests.

Features include the compression  of the embedded data, encryption of the embedded data and automatic integrity checking using a checksum. The JPEG, BMP, WAV and AU file formats are supported for use as cover file. There are no restrictions on the format of the secret data.


## Data Embedding

You can embed the data with the `steghide embed -cf embedding.jpg -ef data.txt` command.

<img width="350" src="https://github.com/wasny0ps/CyberSecurity-Notes/blob/main/OSINT/Steghide/source/embed.png">


## Extract Data

You can extract the data from  file with the `steghide extract -sf embedding.jpg` command.

<img width="350" src="https://github.com/wasny0ps/CyberSecurity-Notes/blob/main/OSINT/Steghide/source/extract.png">


**_by wasny0ps_**
