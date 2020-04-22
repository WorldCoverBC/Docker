# Copyright (C) 2020 Brockmann Consult GmbH (info@brockmann-consult.de)
#
# This program is free software; you can redistribute it and/or modify it
# under the terms of the GNU General Public License as published by the Free
# Software Foundation; either version 3 of the License, or (at your option)
# any later version.
# This program is distributed in the hope that it will be useful, but WITHOUT
# ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or
# FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for
# more details.
#
# You should have received a copy of the GNU General Public License along
# with this program; if not, see http://www.gnu.org/licenses/

FROM centos:7

LABEL version="0.2"
LABEL description="CentOS Linux with MAJA 3.3.5."

# Install conda
RUN rpm --import https://repo.anaconda.com/pkgs/misc/gpgkeys/anaconda.asc
ADD conda.repo /etc/yum.repos.d/conda.repo
RUN yum -y install conda
RUN yum -y install file
RUN yum -y install git
RUN yum -y install sudo
RUN yum -y install unzip

# Create directory structure
RUN mkdir -p /worldcover/maja
RUN mkdir -p /worldcover/cams
RUN mkdir -p /worldcover/gipp
RUN mkdir -p /worldcover/srtm
RUN mkdir -p /worldcover/swbd
RUN mkdir -p /worldcover/dtm
RUN mkdir -p /worldcover/lut
RUN mkdir -p /worldcover/map
RUN mkdir -p /worldcover/tmp
RUN mkdir -p /worldcover/source
RUN mkdir -p /worldcover/target

# Change working directory
WORKDIR /worldcover

# Install MAJA
ADD MAJA-3.3.5.run MAJA-3.3.5.run
RUN ./MAJA-3.3.5.run --target /worldcover/maja

# Install Start-MAJA
RUN git clone https://github.com/CNES/Start-MAJA.git

# Create symboloic links for Start-MAJA
RUN ln -s /worldcover/dtm Start-MAJA/DTM
RUN ln -s /worldcover/source source/maja

# Install S-2 tile map
ADD map/S2A_OPER_GIP_TILPAR_MPC__20151209T095117_V20150622T000000_21000101T000000_B00.kml map/S2A_OPER_GIP_TILPAR_MPC__20151209T095117_V20150622T000000_21000101T000000_B00.kml

# Add scripts
ADD scripts/maja-cleanup maja-cleanup
ADD scripts/maja-create-dtm maja-create-dtm
ADD scripts/maja-start maja-start
ADD scripts/maja-s2-pp maja-s2-pp

# Add MAJA folders
ADD maja-folders.txt maja-folders.txt

# Add MAJA Python environment
ADD maja-env-create maja-env-create
ADD maja-env.yml maja-env.yml

# Add user 'worldcover'
# RUN adduser worldcover
# RUN chown -R worldcover /worldcover
# RUN chgrp -R worldcover /worldcover
# Change user
# USER worldcover

# Create MAJA Python environment
RUN ./maja-env-create

# Cleanup
RUN rm -rf ./maja-env-create
RUN rm -rf ./maja-env.yml
RUN rm -rf ./Start-MAJA/.git
RUN rm -rf ./MAJA-3.3.5.run

# Default command
CMD ["./maja-s2-pp"]
