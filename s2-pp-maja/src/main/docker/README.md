# Create the Docker image

Follow the instructions in

    MAJA-3.3.5.md

to download MAJA and the instructions in

    map/S2A_OPER_GIP_TILPAR_MPC__20151209T095117_V20150622T000000_21000101T000000_B00.md

to download the Sentinel-2 tiling grid KML file. Then create the custom Docker image by typing

    docker build -t bc/worldcover-maja-s2-pp:0.2 . | tee build.log

Test the container and obtain a usage message by typing

    docker run bc/worldcover-maja-s2-pp:0.2

# Save the Docker image

    docker save bc/worldcover-maja-s2-pp:0.2 | gzip > image-bc-worldcover-maja-s2-pp-v0.2.tar.gz

# Load the Docker image 

    zcat image-bc-worldcover-maja-s2-pp-v0.2.tar.gz | docker load

# Run commands within the Docker container

Run the Sentinel-2 MAJA pre-processing on a given source tile

    docker run --memory-reservation 8g \
      --mount type=bind,source=</path/on/host/to/gipp>,destination=/worldcover/gipp \
      --mount type=bind,source=</path/on/host/to/srtm>,destination=/worldcover/srtm \
      --mount type=bind,source=</path/on/host/to/swbd>,destination=/worldcover/swbd \
      --mount type=bind,source=</path/on/host/to/dtm>,destination=/worldcover/dtm \
      --mount type=bind,source=</path/on/host/to/lut>,destination=/worldcover/lut \
      --mount type=bind,source=</path/on/host/to/tmp>,destination=/worldcover/tmp \
      --mount type=bind,source=</path/on/host/to/source>,destination=/worldcover/source \
      --mount type=bind,source=</path/on/host/to/target>,destination=/worldcover/target \
      bc/worldcover-maja-s2-pp:0.2 \
        ./maja-s2-pp <tile> [<site>] [<start-date>] [<final-date>]

Be sure to replace the items in <chevrons> with paths and file names, which actually exist. For example:

    docker run \
      --mount type=bind,source=$HOME/worldcover-example/maja/gipp,destination=/worldcover/gipp \
      --mount type=bind,source=/$HOME/worldcover-example/srtm,destination=/worldcover/srtm \
      --mount type=bind,source=$HOME/worldcover-example/swbd,destination=/worldcover/swbd \
      --mount type=bind,source=$HOME/worldcover-example/maja/dtm,destination=/worldcover/dtm \
      --mount type=bind,source=$HOME/worldcover-example/maja/lut,destination=/worldcover/lut \
      --mount type=bind,source=$HOME/worldcover-example/maja/tmp,destination=/worldcover/tmp \
      --mount type=bind,source=$HOME/worldcover-example/source,destination=/worldcover/source \
      --mount type=bind,source=$HOME/worldcover-example/target,destination=/worldcover/target \
      bc/worldcover-maja-s2-pp:0.2 \
        ./maja-s2-pp 32UME Bremen

# Example data

If you are member of the Worldcover project team, you can find the data necessary to run the above example at

    ftp://cvbftp.vgt.vito.be/exchange/S2_processing/MAJA

A Docker image resides in the same directory. Unzip the archives included with the

    worldcover-example
  
directory before you run the example.

# Timing results

Running the above example needed 26701 seconds.

# Using MAJA for regions not covered by SRTM

There is an [issue and solution](https://github.com/CNES/Start-MAJA/issues/53) on the Start-MAJA GitHub repository related to this problem.

