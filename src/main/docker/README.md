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
    
Runs the Sentinel-2 pre-processing on a given source tile

    docker run \
      --mount type=bind,source=</path/on/host/to/dem>,destination=/home/worldcover/processing/dem \
      --mount type=bind,source=</path/on/host/to/source>,destination=/home/worldcover/processing/source \
      --mount type=bind,source=</path/on/host/to/target>,destination=/home/worldcover/processing/target \
      bc/worldcover-s2-pp:0.3 ./worldcover-s2-pp \
        <dem-tile-name> <source-tile-name> <target-tile-name>

Be sure to replace the items in `<chevrons>` with paths and file names, which actually exist. For example:

    docker run --mount type=bind,source=$HOME/T32UNE/dem,destination=/home/worldcover/processing/dem \
      --mount type=bind,source=$HOME/T32UNE/source,destination=/home/worldcover/processing/source \
      --mount type=bind,source=$HOME/T32UNE/target,destination=/home/worldcover/processing/target \
      bc/worldcover-s2-pp:0.3 ./worldcover-s2-pp \
        dem_32UNE.tif S2A_MSIL1C_20191007T103021_N0208_R108_T32UNE_20191007T123034.SAFE test.tif
