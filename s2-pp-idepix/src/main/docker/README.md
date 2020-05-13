# Create the Docker image

Create the custom Docker image by typing

    docker build -t bc/worldcover-idepix-s2-pp:0.8 . | tee build.log

Test the `gpt` command

    docker run bc/worldcover-idepix-s2-pp:0.8 snap/bin/gpt

# Save the Docker image

    docker save bc/worldcover-idepix-s2-pp:0.8 | gzip > image-bc-worldcover-idepix-s2-pp-v0.8.tar.gz

# Load the Docker image 

    zcat image-bc-worldcover-idepix-s2-pp-v0.8.tar.gz | docker load

# Run commands within the Docker container

Display the Sentinel-2 pre-processing command usage message

    docker run bc/worldcover-idepix-s2-pp:0.8
    
Run the Sentinel-2 pre-processing on a given source tile

    docker run --memory-reservation 8g \
      --mount type=bind,source=</path/on/host/to/dem>,destination=/home/worldcover/processing/dem \
      --mount type=bind,source=</path/on/host/to/source>,destination=/home/worldcover/processing/source \
      --mount type=bind,source=</path/on/host/to/target>,destination=/home/worldcover/processing/target \
      bc/worldcover-idepix-s2-pp:0.8 ./worldcover-s2-pp \
        <target-resolution> <dem-tile-file-name> <msi-tile-file-name> <target-tile-file-name> <target-resolution>

Be sure to replace the items in `<chevrons>` with paths and file names, which actually exist. For example:

    docker run --memory-reservation 8g \
    --mount type=bind,source=$HOME/test/dem,destination=/home/worldcover/processing/dem \
    --mount type=bind,source=$HOME/test/source,destination=/home/worldcover/processing/source \
    --mount type=bind,source=$HOME/test/target,destination=/home/worldcover/processing/target \
    bc/worldcover-idepix-s2-pp:0.8 ./worldcover-s2-pp \
      dem_32UNE.tif S2A_MSIL1C_20191007T103021_N0208_R108_T32UNE_20191007T123034.SAFE dst_32UNE_20m.tif 20m

or

    docker run --memory-reservation 8g \
    --mount type=bind,source=$HOME/test/dem,destination=/home/worldcover/processing/dem \
    --mount type=bind,source=$HOME/test/source,destination=/home/worldcover/processing/source \
    --mount type=bind,source=$HOME/test/target,destination=/home/worldcover/processing/target \
    bc/worldcover-idepix-s2-pp:0.8 ./worldcover-s2-pp \
      dem_32UNE.tif S2A_MSIL1C_20191007T103021_N0208_R108_T32UNE_20191007T123034.SAFE dst_32UNE_60m.tif 60m

# Example data

You can find the data from the above example at

    ftp://cvbftp.vgt.vito.be/exchange/S2_processing/T32UNE.tar.gz

if you are member of the Worldcover project team. A Docker image resides in the same directory.

# Timing results

| CPUs | Target resolution | Minutes | Seconds  | Total seconds |
|:----:|:-----------------:| -------:| --------:| -------------:|
| 2    | 20 m              | -       | -        | -             |
| 2    | 60 m              | -       | -        | -             |
| 4    | 20 m              | 13      | 19.41    | 800           |
| 4    | 60 m              | 4       | 18.63    | 258           |
| 8    | 20 m              | -       | -        | -             |
| 8    | 60 m              | -       | -        | -             |

The above timing results were obtained on `uservm.terrascope.be` (CentOS, 4 CPU cores, 8 GB RAM) running the commands

    nohup time ./worldcover-s2-pp dem_32UNE.tif S2A_MSIL1C_20191007T103021_N0208_R108_T32UNE_20191007T123034.SAFE test_20m.tif 20m &

    nohup time ./worldcover-s2-pp dem_32UNE.tif S2A_MSIL1C_20191007T103021_N0208_R108_T32UNE_20191007T123034.SAFE test_60m.tif 60m &
