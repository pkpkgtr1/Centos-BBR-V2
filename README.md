# Centos-BBR-V2
# 方法一
wget https://github.com/pkpkgtr1/Centos-BBR-V2/releases/download/v1.0/bbr_kernel.zip  
yum -y install unzip && unzip bbr_kernel.zip  
yum -y localinstall *  
grub2-set-default 0  
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf  
echo "net.ipv4.tcp_congestion_control = bbr2" >> /etc/sysctl.conf  
echo "net.ipv4.tcp_ecn = 1" >> /etc/sysctl.conf   # 开启ecn (可选)  
sysctl -p  
rm kernel-5.2.0_rc3+-1.x86_64.rpm  
rm kernel-headers-5.2.0_rc3+-1.x86_64.rpm  
reboot  



--------------------------------------------------------------------------------------------
# 方法二（如果方法一无法成功安装内核请用此方法）
yum -y install "https://wget.es/Kernel/Centos/kernel-5.4.0_rc6-1.x86_64.rpm"  # 安装内核  
awk -F\' '$1=="menuentry " {print i++ " : " $2}' /etc/grub2.cfg     # 查看Grub2菜单  
grub2-set-default 0  # 选择默认引导项  
# 启用BBR2
sed -i '/net.core.default_qdisc/d' /etc/sysctl.conf  
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf  
sed -i '/net.ipv4.tcp_congestion_control/d' /etc/sysctl.conf  
echo "net.ipv4.tcp_congestion_control=bbr2" >> /etc/sysctl.conf  
sed -i '/net.ipv4.tcp_ecn/d' /etc/sysctl.conf  
echo "net.ipv4.tcp_ecn=1" >> /etc/sysctl.conf # 启用ECN（不想启用就不要执行这一个） 
reboot
