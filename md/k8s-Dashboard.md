<!--
 * @Author: tangdaoyong
 * @Date: 2021-01-27 09:37:21
 * @LastEditors: tangdaoyong
 * @LastEditTime: 2021-01-27 13:37:22
 * @Description: Kubernetes Dashboard
-->
# Kubernetes Dashboard

`Kubernetes Dashboard`是`k8s`的可视化插件。

[kubernetes部署dashboard可视化插件](https://blog.csdn.net/networken/article/details/85607593)
[Kubernetes Dashboard部署](https://blog.csdn.net/weixin_38042339/article/details/109143917)

部署 Kubernetes Dashboard
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended.yaml
#开启本机访问代理
$ kubectl proxy

创建Dashboard管理员用户并用token登陆
# 创建 ServiceAccount kubernetes-dashboard-admin 并绑定集群管理员权限
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml

# 获取登陆 token
$ kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep kubernetes-dashboard-admin | awk '{print $1}')


-------

部署 Kubernetes dashboard
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.4/aio/deploy/recommended.yaml
或

kubectl create -f kubernetes-dashboard.yaml
检查 kubernetes-dashboard 应用状态

kubectl get pod -n kubernetes-dashboard
开启 API Server 访问代理

kubectl proxy
通过如下 URL 访问 Kubernetes dashboard

http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy

http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login

配置控制台访问令牌
对于Mac环境

TOKEN=$(kubectl -n kube-system describe secret default| awk '$1=="token:"{print $2}')
kubectl config set-credentials docker-for-desktop --token="${TOKEN}"
echo $TOKEN
对于Windows环境

$TOKEN=((kubectl -n kube-system describe secret default | Select-String "token:") -split " +")[1]
kubectl config set-credentials docker-for-desktop --token="${TOKEN}"
echo $TOKEN

如：
```bash
TOKEN=$(kubectl -n kube-system describe secret default| awk '$1=="token:"{print $2}')
kubectl config set-credentials docker-for-desktop --token="${TOKEN}"
echo $TOKEN
User "docker-for-desktop" set.
eyJhbGciOiJSUzI1NiIsImtpZCI6InpaUFpkMFBGM3NQVFB3RzFrdDVlZUlmWDExS2JfaVhKWDZTRjhjenFHYkkifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJkZWZhdWx0LXRva2VuLXQ2cHp2Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImRlZmF1bHQiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiJjNmM1ZjQwYi0xMGVlLTRhMDQtOGYxYy03ZDg3OWQ3NTZlZTAiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZS1zeXN0ZW06ZGVmYXVsdCJ9.eeCkkY77vB4OwIWoGoKDDmKWIfgFxw6JknQ9Kv_guXL9eWGLtlNxvEk1kMsNcZ4vfO4HYVBclRqWwIWW4NiV-im055Ey924YYJMOB6TPwbwzKfjvqavIuDA-yfu1sv7D3echhlgwGwIMJff82qJ7lUyrts0FUgrswUA-E5dDqm6Z5ceWNl-mQCVFq6TNHFuRDeGgFUYZsEAG5laCUVhamc340CuqkDogiHWsk4U3rNWZdtUlUsluPNz-gzRMfehz4lc4FZCAs8k6jmbaJMcvaiL6uepAraJggGTxXhIGlw3ftw3RcPpHPTGrTVqEqVs1YrF3RrTLZPgeize1a2Tt4w
```
输入上文控制台输出的内容

或者选择 Kubeconfig 文件,路径如下：

Mac: $HOME/.kube/config
Win: %UserProfile%\.kube\config
点击登陆，进入Kubernetes Dashboard