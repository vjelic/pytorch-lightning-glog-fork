FROM rocm/pytorch:latest

LABEL maintainer="PyTorchLightning <https://github.com/PyTorchLightning>"

RUN \
git clone https://github.com/Lightning-AI/lightning.git && \
git clone https://github.com/pallets/click.git && \
cd click && \
python setup.py install && \ 
cd .. && \
git clone https://github.com/seperman/deepdiff.git && \
cd deepdiff && \
python setup.py install && \
cd .. && \
# git clone https://github.com/tiangolo/fastapi.git && \ 
# cd fastapi && \ 
# python setup.py install && \ 
# cd .. && \
pip install fastapi && \
pip install lightning-cloud && \
git clone https://github.com/pallets/jinja.git && \ 
cd jinja && \ 
python setup.py install && \ 
cd .. && \ 
pip install starsessions && \ 
pip uninstall fsspec && \ 
pip install fsspec==2022.1.0 && \ 
git clone https://github.com/omni-us/jsonargparse.git && \ 
cd jsonargparse && \ 
python setup.py install && \ 
cd .. && \ 
#needs docstring_parser-0.14.1
pip install -U jsonargparse[signatures] && \
cd lightning && \ 
python setup.py install 