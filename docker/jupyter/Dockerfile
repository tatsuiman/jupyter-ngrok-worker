# Jupyterのベースイメージを使用
FROM jupyter/base-notebook

# jupyter_server_config.pyを作成または編集
RUN echo "c.MappingKernelManager.cull_idle_timeout = 600  # 10 minutes in seconds" >> ~/.jupyter/jupyter_server_config.py \
    && echo "c.MappingKernelManager.cull_interval = 60  # Check every minute" >> ~/.jupyter/jupyter_server_config.py \
    && echo "c.MappingKernelManager.cull_connected = True  # Even if a browser tab is open" >> ~/.jupyter/jupyter_server_config.py
