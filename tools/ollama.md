# Ollama

## Install

curl https://ollama.ai/install.sh | sh

ollama pull llama3

## Config

Use the GPU Intel Corporation HD Graphics 620 (rev 02

lspci -k | grep -EA3 'VGA|3D|Display'

00:02.0 VGA compatible controller: Intel Corporation HD Graphics 620 (rev 02)
        DeviceName: Onboard IGD
        Subsystem: Hewlett-Packard Company Device 827d
        Kernel driver in use: i915

# Set environment variables for better performance but seems to be worse performance

export OLLAMA_NUM_GPU=999

export ONEAPI_DEVICE_SELECTOR=level_zero:0

./start-ollama.sh


## Run

./ollama run llama3


