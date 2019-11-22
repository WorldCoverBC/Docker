# Create the Docker image

Create the custom Docker image by typing

    docker build -t bc/worldcover-s2-pp:0.1 . | tee build.log

Test the `gpt` command

    docker run bc/worldcover-s2-pp:0.1 snap/bin/gpt

# Save the Docker image

    docker save bc/worldcover-s2-pp:0.1 | gzip > image-bc-worldcover-s2-pp-v0.1.tar.gz

# Load the Docker image 

    zcat image-bc-worldcover-s2-pp-v0.1.tar.gz | docker load

# Run commands within a Docker container

    docker run bc/worldcover-s2-pp:0.1

displays the SNAP command line tool help message

    docker run bc/worldcover-s2-pp:0.1 \
      --mount type=bind,source=</path/on/host/to/dem>,destination=/home/worldcover/processing/dem \
      --mount type=bind,source=</path/on/host/to/source>,destination=/home/worldcover/processing/source \
      --mount type=bind,source=</path/on/host/to/target>,destination=/home/worldcover/processing/target \ 
      source-product-file-name target-product-file-name

runs the Sentinel-2 pre-processing steps on a given source product file.
