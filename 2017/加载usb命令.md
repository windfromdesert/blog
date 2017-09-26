使用 `iocharset` 设置编码，使用 `umask` 设置权限（ 000 代表所有用户具有读写执行权限）

比如：

    sudo mount -o iocharset=utf8,umask=000 /dev/sdb1 /mnt/usb
