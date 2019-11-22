# Copyright (C) 2019 Brockmann Consult GmbH (info@brockmann-consult.de)
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

FROM ubuntu:bionic

LABEL version="0.1"
LABEL description="Ubuntu Linux with Sentinel Toolboxes, IdePix and Sentinel-2 atmospheric correction."

# Update the Linux system and install prerequisites
RUN apt-get -y update
RUN apt-get -y upgrade
RUN apt-get install -y wget
RUN apt-get autoremove
RUN apt-get autoclean

# Create user "worldcover"
RUN useradd --create-home worldcover

# Execute all commands below as user "worldcover"
USER worldcover

WORKDIR /home/worldcover
RUN mkdir -p processing/dem
RUN mkdir -p processing/source
RUN mkdir -p processing/target

# Fetch the SNAP installer script from ESA
RUN wget http://step.esa.int/downloads/7.0/installers/esa-snap_sentinel_unix_7_0.sh

# Add scripts for updating and installing modules
ADD --chown=worldcover esa-snap-update-modules esa-snap-update-modules
ADD --chown=worldcover esa-snap-install-module esa-snap-install-module
ADD --chown=worldcover esa-snap-install-modules esa-snap-install-modules
ADD --chown=worldcover modules-install.txt modules-install.txt

# Add GPT wrapper script and graph
ADD --chown=worldcover worldcover-s2-pp worldcover-s2-pp
ADD --chown=worldcover worldcover-s2-pp.xml worldcover-s2-pp.xml

# Make all scripts executable
RUN chmod ug+x esa-snap*

# Run SNAP installation script
RUN ./esa-snap_sentinel_unix_7_0.sh -q

# Update all SNAP modules
RUN ./esa-snap-update-modules

# Install additional modules
RUN ./esa-snap-install-modules

# Customize Java VM settings
RUN rm -f snap/bin/gpt.vmoptions
ADD --chown=worldcover gpt.vmoptions snap/bin/gpt.vmoptions

# Cleanup
RUN rm -f esa-snap*

# Environment
ENV LD_LIBRARY_PATH .

# Run SNAP command line tool (for initialization only)
RUN snap/bin/gpt

# Default command
CMD ["snap/bin/gpt"]
