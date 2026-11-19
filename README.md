# init-node — 跨发行版 Linux 节点初始化

用 Ansible Role 一键初始化裸机：换源、EPEL、SELinux、防火墙、node_exporter。

## 功能
- Rocky 系：dnf 装 EPEL/htop、SELinux disabled、firewalld→iptables、二进制部署 node_exporter（systemd 模板 + handler）
- Ubuntu 系：apt 换阿里云源、安装 prometheus-node-exporter
- 按 ansible_os_family 自动分派（when + import_tasks）

## 使用
1. 配置 inventory/hosts.ini（SSH 密钥 + sudo 免密）
2. ansible-playbook site.yml -i inventory/hosts.ini
3. 验证：curl localhost:9100/metrics

## 配套监控
Prometheus + Alertmanager（QQ 邮箱告警）+ Grafana，三节点监控面板

## 排障案例（实战沉淀）
- dpkg lock 被占：fuser 定位持锁进程，禁 unattended-upgrades 三层结构
- SSH 公钥认证失败：ssh -v + ssh-keygen -lf 指纹对比定位粘贴损坏
- VS Code ripgrep 进程 CPU 失控：告警→top 定位→kill→告警自动恢复

