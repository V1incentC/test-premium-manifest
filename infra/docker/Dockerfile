FROM ghcr.io/zephyrproject-rtos/ci:v0.28.7

# 1. Install Nordic Tools
WORKDIR /tmp
RUN wget -qO - https://nsscprodmedia.blob.core.windows.net/prod/software-and-other-downloads/desktop-software/nrf-command-line-tools/sw/versions-10-x-x/10-24-0/nrf-command-line-tools_10.24.0_amd64.deb > nrf-tools.deb \
    && dpkg -i nrf-tools.deb \
    && rm nrf-tools.deb

# 2. Fetch Requirements for NCS v3.1.1
WORKDIR /workdir
RUN west init -m https://github.com/nrfconnect/sdk-nrf --mr v3.1.1 \
    && west update --narrow -o=--depth=1 \
    && pip3 install -r zephyr/scripts/requirements.txt \
    && pip3 install -r nrf/scripts/requirements.txt \
    && pip3 install -r bootloader/mcuboot/scripts/requirements.txt

# 3. Cleanup
RUN rm -rf /workdir/*

# 4. Environment Setup
ENV ZEPHYR_TOOLCHAIN_VARIANT=zephyr
ENV LC_ALL=C.UTF-8
ENV LANG=C.UTF-8

# --- THE CLEAN FIX ---
# Use the variable provided by the base image (ZSDK_VERSION) to find the path
ENV ZEPHYR_SDK_INSTALL_DIR=/opt/toolchains/zephyr-sdk-${ZSDK_VERSION}
ENV PATH="${ZEPHYR_SDK_INSTALL_DIR}/arm-zephyr-eabi/bin:${PATH}"