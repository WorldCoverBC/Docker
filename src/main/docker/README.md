# Create the Docker image

Create the custom Docker image by typing

    docker build -t bc/worldcover-s2-pp:0.2 . | tee build.log

Test the `gpt` command

    docker run bc/worldcover-s2-pp:0.2 snap/bin/gpt

# Save the Docker image

    docker save bc/worldcover-s2-pp:0.2 | gzip > image-bc-worldcover-s2-pp-v0.2.tar.gz

# Load the Docker image 

    zcat image-bc-worldcover-s2-pp-v0.2.tar.gz | docker load

# Run commands within the Docker container

Display the SNAP command line tool help message

    docker run bc/worldcover-s2-pp:0.2

Display the Sentinel-2 pre-processing command usage message

    docker run bc/worldcover-s2-pp:0.2 ./worldcover-s2-pp
    
Runs the Sentinel-2 pre-processing on a given source product file

    docker run bc/worldcover-s2-pp:0.2 \
      --mount type=bind,source=</path/on/host/to/dem>,destination=/home/worldcover/processing/dem \
      --mount type=bind,source=</path/on/host/to/source>,destination=/home/worldcover/processing/source \
      --mount type=bind,source=</path/on/host/to/target>,destination=/home/worldcover/processing/target \ 
      ./worldcover-s2-pp <source-product-file-name> <target-product-file-name>

Be sure to replace the items in `<chevrons>` with paths and file names, which actually exist.
