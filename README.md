# 卡块处理

### 大文件克隆需要安装[git lfs](https://git-lfs.com/)

### 如何手动从好的节点到处区块（--from 参数后面的数字15974855是你卡块的高度，最好减一）
``` BASH
git clone https://github.com/Smallwolf2021/crust-import-blocks.git
cd crust-import-blocks
chmod +x ./crust
./crust export-blocks --base-path /opt/crust/data/chain --chain mainnet --pruning archive --binary --from 15974855 newblocks

```
- 使用任何你熟练的方法将导出的区块上传至卡块的节点中
  - 本地搭建web或tftp服务
  - Windows使用WINSCP/IIS/TFTP32

### 解压区块
``` BASH
tar -xvzf blocks/newblocks.tar.gz
```


### 如何导入区块
``` BASH
./crust import-blocks --base-path /opt/crust/data/chain --chain mainnet --pruning archive --wasm-execution Compiled --binary newblocks

```

#### 导入过程可能会比较缓慢，如果区块数量不多耐心等待即可，如果数量太大，可以导入部分后退出重新启动节点，尝试是否正常开始同步

#### 其他
- [简易区块节点监控程序](./docs/daemon.md)