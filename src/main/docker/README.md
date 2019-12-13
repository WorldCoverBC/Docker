# Create the Docker image

Create the custom Docker image by typing

    docker build -t bc/worldcover-s2-pp:0.3 . | tee build.log

Test the `gpt` command

    docker run bc/worldcover-s2-pp:0.3 snap/bin/gpt

# Save the Docker image

    docker save bc/worldcover-s2-pp:0.3 | gzip > image-bc-worldcover-s2-pp-v0.3.tar.gz

# Load the Docker image 

    zcat image-bc-worldcover-s2-pp-v0.3.tar.gz | docker load

# Run commands within the Docker container

Display the SNAP command line tool help message

    docker run bc/worldcover-s2-pp:0.3

Display the Sentinel-2 pre-processing command usage message

    docker run bc/worldcover-s2-pp:0.3 ./worldcover-s2-pp
    
Run the Sentinel-2 pre-processing on a given source tile

    docker run --memory-reservation 16g \
      --mount type=bind,source=</path/on/host/to/dem>,destination=/home/worldcover/processing/dem \
      --mount type=bind,source=</path/on/host/to/source>,destination=/home/worldcover/processing/source \
      --mount type=bind,source=</path/on/host/to/target>,destination=/home/worldcover/processing/target \
      bc/worldcover-s2-pp:0.3 ./worldcover-s2-pp \
        <target-resolution> <dem-tile-file-name> <msi-tile-file-name> <target-tile-file-name>

Be sure to replace the items in `<chevrons>` with paths and file names, which actually exist. For example:

    docker run --memory-reservation 16g \
      --mount type=bind,source=$HOME/test/dem,destination=/home/worldcover/processing/dem \
      --mount type=bind,source=$HOME/test/source,destination=/home/worldcover/processing/source \
      --mount type=bind,source=$HOME/test/target,destination=/home/worldcover/processing/target \
      bc/worldcover-s2-pp:0.3 ./worldcover-s2-pp \
        10m dem_32UNE.tif S2A_MSIL1C_20191007T103021_N0208_R108_T32UNE_20191007T123034.SAFE dst_32UNE_10m.tif

or

    docker run --memory-reservation 16g \
      --mount type=bind,source=$HOME/test/dem,destination=/home/worldcover/processing/dem \
      --mount type=bind,source=$HOME/test/source,destination=/home/worldcover/processing/source \
      --mount type=bind,source=$HOME/test/target,destination=/home/worldcover/processing/target \
      bc/worldcover-s2-pp:0.3 ./worldcover-s2-pp \
        20m dem_32UNE.tif S2A_MSIL1C_20191007T103021_N0208_R108_T32UNE_20191007T123034.SAFE dst_32UNE_20m.tif

# Example data

You can find the data from the above example at

    ftp://cvbftp.vgt.vito.be/exchange/S2_processing/T32UNE.tar.gz

if you are member of the Worldcover project team. A Docker image resides in the same directory.
