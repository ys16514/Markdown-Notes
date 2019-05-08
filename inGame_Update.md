# ingame 更新流程

* Jenkins 执行 “内部测试服比赛更新”

* 将 `106.14.29.185` 上的 `/trunk/gas_game/` 打包成 `gas_game.tar.gz`
    
    ```
    tar -vczf gas_game.tar.gz gas_game/
    ```

* 将打包文件下载到本地
    
    ```
    sz gas_game.tar.gz
    ```

* 将打包文件上传到 `192.168.131.175` 上的 `hzhexiekai/docker_compose/` 路径下

    ```
    rz
    ```

* 解压文件，覆盖 `gas_game/` 文件夹

    ```
    tar -zxvf gas_game.tar.gz
    ```