#### 磁盘阵列组

1. <img src="C:\Users\86184\AppData\Roaming\Typora\typora-user-images\image-20210403205236929.png" alt="image-20210403205236929" style="zoom:50%;" /><img src="C:\Users\86184\AppData\Roaming\Typora\typora-user-images\image-20210403205441790.png" alt="image-20210403205441790" style="zoom: 80%;" />

2. <img src="C:\Users\86184\AppData\Roaming\Typora\typora-user-images\image-20210403205254995.png" alt="image-20210403205254995" style="zoom:50%;" /><img src="C:\Users\86184\AppData\Roaming\Typora\typora-user-images\image-20210403205517492.png" alt="image-20210403205517492" style="zoom:67%;" />

3. <img src="C:\Users\86184\AppData\Roaming\Typora\typora-user-images\image-20210403205327471.png" alt="image-20210403205327471" style="zoom: 33%;" /><img src="C:\Users\86184\AppData\Roaming\Typora\typora-user-images\image-20210403205538978.png" alt="image-20210403205538978" style="zoom: 67%;" />

   

> ```
> 1. mdadm -Cv /dev/md0 -a yes -n 4 -l 10 /dev/sdb /dev/sdc /dev/sdd /dev/sde
> ```

* -C参数代表创建一个RAID阵列卡；-v参数显示创建的过程，同时在后面追加一个设备名称/dev/md0，这样/dev/md0就是创建后的RAID磁盘阵列的名称；-a yes参数代表自动创建设备文件；-n 4参数代表使用4块硬盘来部署这个RAID磁盘阵列；而-l 10参数则代表RAID 10方案；最后再加上4块硬盘设备的名称就搞定了

> 2. 其次，把制作好的RAID磁盘阵列格式化为ext4格式

* mkfs.ext4 /dev/md0  //采用ext4格式

  

  > 3. 最后，查看/dev/md0磁盘阵列的详细信息，并把挂载信息写入到配置文件中，使其永久生效

  *mdadm -D /dev/md0*

  *echo "/dev/md0 /RAID ext4 defaults 0 0" >> /etc/fstab*

  

  #### SSH
  
  
  
  ### 防火墙
  
  > ![image-20210404204955088](C:\Users\86184\AppData\Roaming\Typora\typora-user-images\image-20210404204955088.png)
  >
  > 防火墙开启telnet 23号端口
  >
  > telnet IP    //telnet 连接服务
  
  #### debain
  
  > ![image-20210409154019251](C:\Users\86184\AppData\Roaming\Typora\typora-user-images\image-20210409154019251.png)