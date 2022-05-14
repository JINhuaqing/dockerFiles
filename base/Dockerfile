FROM continuumio/anaconda3 
MAINTAINER Jin<kevinjin0423@gmail.com>

ENV MYPATH /root
WORKDIR $MYPATH


RUN apt-get update -y \ 
    && apt-get upgrade -y \ 
    && apt-get install r-base -y \
    && apt-get install vim -y \
    && apt-get install tmux -y  

# config files
RUN git clone https://github.com/JINhuaqing/software_conf.git  \ 
    && cp software_conf/vim/linux/vimrc_basic ~/.vimrc  \ 
    && cp software_conf/tmux/linux/.tmux.conf ~/.tmux.conf \
    && rm -r software_conf

# jupyter ext
RUN pip install jupyter_contrib_nbextensions \
    && jupyter contrib nbextension install --user \
    && pip install jupyter_nbextensions_configurator \
    && jupyter nbextensions_configurator enable --user

RUN mkdir -p $(jupyter --data-dir)/nbextensions \
    &&  cd $(jupyter --data-dir)/nbextensions \
    &&  git clone https://github.com/lambdalisue/jupyter-vim-binding vim_binding \
    &&  jupyter nbextension enable vim_binding/vim_binding 

RUN cd .jupyter/  \
    && jupyter notebook --generate-config \
    && sed -i 's/# c.NotebookApp.port = 8888/c.NotebookApp.port = 8888/' jupyter_notebook_config.py \
    && sed -i 's/# c.NotebookApp.open_browser = True/c.NotebookApp.open_browser = False/' jupyter_notebook_config.py \
    && sed -i 's/# c.NotebookApp.allow_root = False/c.NotebookApp.allow_root = True/' jupyter_notebook_config.py  \
    && sed -i "s/# c.NotebookApp.ip = 'localhost'/c.NotebookApp.ip = '*'/" jupyter_notebook_config.py  \
    && cd ..

EXPOSE 8888
EXPOSE 22

CMD /bin/bash

