### arch遇见的bug

蓝牙没法用：

❯ bluetoothctl power on
No default controller available

据说是6.9.1内核的bug，也许在6.9.2能够恢复，暂时能够用符号链接解决，其实就是缺了下面那个文件，但是可以用上面的文件替代，所以把下面的文件名和上面的文件对应起来就行了：

```
sudo ln -s /lib/firmware/mediatek/BT_RAM_CODE_MT7961_1_2_hdr.bin.zst /lib/firmware/mediatek/BT_RAM_CODE_MT7961_1a_2_hdr.bin.zst
```

但是需要注意创建符号链接会阻止arch更新与之相关的包，所以下次更新时记得删除符号链接，可以用rm或者unlink，推荐unlink，rm可能误删重要文件。