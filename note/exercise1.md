- task
    - 新建一个bash文件，在GPU上成功运行
        
        bash主要功能：
        
        - 新建虚拟环境
        - 运行代码
        - 实现hyper parameter search：
            - 改变参数的不同的值，主要是learning rate和train_batch_size
            - 记录日志：主要是validation loss 和hyper parameters的值
        - 看yaml文件和原来项目里面的yaml文件有什么不同
- need to git
    - bash脚本
    - 日志
    - yaml file
    
- 本地experiments
    - 新建虚拟环境 python3.8
    - 安装requirements
    - no module named ‘yaml’， 运行pip install yaml 报错
    
    ERROR: Could not find a version that satisfies the requirement yaml (from versions: none)
    ERROR: No matching distribution found for yaml
    
    - 后来发现要安装pyyaml包，pip install pyyaml
    - 运行python [train.py](http://train.py/) --config config.yaml
    
    - 结果
    
    ```bash
    
    ```
    
- 服务器上experiments
    
    building_env.sh文件内容如下
    
    ```bash
    ENVNAME=word2vec3.8 
    conda create -n $ENVNAME python=3.8 &&
    source ~/anaconda3/etc/profile.d/conda.sh &&
    conda activate $ENVNAME &&
    
    rm -rf torch-1.11.0+cu115-cp38-cp38-linux_x86_64.whl
    wget https://download.pytorch.org/whl/cu115/torch-1.11.0%2Bcu115-cp38-cp38-linux_x86_64.whl &&
    pip install torch-1.11.0+cu115-cp38-cp38-linux_x86_64.whl &&
    
    # before git clone, add your ssh key to github.com
    rm -rf word2vec-pytorch
    git clone git@github.com:ShengdingHu/word2vec-pytorch.git &&
    cd word2vec-pytorch &&
    pip install -r requirements.txt &&
    
    rm -rf weights/ &&
    python train.py --config config.yaml  # the first time might be slow
    ```
    
    觉得上面还有改进的地方，例如在conda create env的时候，不需要自己输入y。
    
    - 运行./building_env.sh，出现错误。添加权限之后，就开始安装了
    
    ```bash
    ./building_env.sh
    -bash: ./building_env.sh: Permission denied
    (base) gpu:~/user/word2vec-pytorch/word2vec-pytorch$ chmod 777 building_env.sh
    ```
    
    - 出现了错误
    
    ERROR: torch-1.11.0+cu115-cp38-cp38-linux_x86_64.whl is not a supported wheel on this platform.
    
    (我觉得这是因为cuda版本选错了，GPU是114版本)
    
    - 出现了错误
    
    ```bash
    Cloning into 'word2vec-pytorch'...
    The authenticity of host 'github.com (20.205.243.166)' can't be established.
    git@github.com: Permission denied (publickey).
    fatal: Could not read from remote repository.
    
    Please make sure you have the correct access rights
    and the repository exists.
    ```
    
    (我觉得是没有加ssh)
    
    ```bash
    (base) gpu:~$ cd ~/.ssh
    (base) gpu:~/.ssh$ ls
    known_hosts
    (base) gpu:~/.ssh$ vim known_hosts
    (base) gpu:~/.ssh$ ssh-keyscan -t rsa github.com >> ~/.ssh/known_hosts
    ```