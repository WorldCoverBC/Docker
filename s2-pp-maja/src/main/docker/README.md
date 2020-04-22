# Create the Docker image

Create the custom Docker image by typing

    docker build -t bc/worldcover-maja-s2-pp:0.1 . | tee build.log

Test the container and obtain a usage message by typing

    docker run bc/worldcover--maja-s2-pp:0.1

# Save the Docker image

    docker save bc/worldcover-maja-s2-pp:0.1 | gzip > image-bc-worldcover-maja-s2-pp-v0.1.tar.gz

# Load the Docker image 

    zcat image-bc-worldcover-maja-s2-pp-v0.1.tar.gz | docker load

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
      bc/worldcover-maja-s2-pp:0.1 \
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
      bc/worldcover-maja-s2-pp:0.1 \
        ./maja-s2-pp 32UME Bremen

# Example data

If you are member of the Worldcover project team, you can find the data necessary to run the above example at

    ftp://cvbftp.vgt.vito.be/exchange/S2_processing/MAJA

A Docker image resides in the same directory. Unzip the archives included with the

    worldcover-example
  
directory before you run the example.

# Timing results

Running the above example to 26701 seconds.
