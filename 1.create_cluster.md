
### 1. 创建EKS管理机器，可选择EC2或者Cloud9

### 2. 创建IAM用户，记录AK、SK。如需控制最小权限请参考FAQ2

### 3. 启动linux机器（或者mac机器），安装/更新aws cli，并配置aws cli

``` shell
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

$ aws configure
AWS Access Key ID : <AK>
AWS Secret Access Key : <SK>
Default region name: <区域代码，如新加坡为ap-southeast-1>
Default output format [None]:json

#测试AK/SK是否生效,有类似如下输出则表示配置生效
aws sts get-caller-identity
{
    "UserId": "AIDAVY4EIOPQFCK******",
    "Account": "397****81984",
    "Arn": "arn:aws:iam::397****81984:user/eksops"
}

# 安装k8s管理工具kubectl https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html
sudo curl --silent --location -o /usr/local/bin/kubectl \
https://s3.us-west-2.amazonaws.com/amazon-eks/1.24.9/2023-01-11/bin/linux/amd64/kubectl
sudo chmod +x /usr/local/bin/kubectl

# 安装eksctl
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv -v /tmp/eksctl /usr/local/bin
eksctl version    # 确认安装成功
```

### 3. 创建集群

```shell
# 参数说明
#--node-type 工作节点类型 默认为m5.large
#--nodes 工作节点数量 默认为2
# --version 
export CLUSTER_NAME=eksworkshop
eksctl create cluster --name=${CLUSTER_NAME} --node-type m6i.large --managed --version 1.23

# 等待以上命令执行完成后（约3~5分钟），执行如下命令确认集群创建成功
kubectl get nodes
NAME                                                STATUS   ROLES    AGE   VERSION
ip-192-168-36-125.ap-northeast-1.compute.internal   Ready    <none>   29h   v1.23.13-eks-fb459a0
ip-192-168-70-130.ap-northeast-1.compute.internal   Ready    <none>   29h   v1.23.13-eks-fb459a0

# 或使用yaml文件创建，请参考
https://www.eksworkshop.com/030_eksctl/launcheks/

```
