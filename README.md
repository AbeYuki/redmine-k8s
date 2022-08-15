# redmine-k8s
redmine with kubernetes


# Quick start
環境ごとに用意されている kustomize があるディレクトリに移動  
```
cd overlay/dev/
```
password ファイル作成
```
echo -n 'password' > secret/password.txt
```
deploy secret
```
kubectl apply -k secret/
```
deploy resources
```
kubectl apply -k ./
```

# Note
migrate に時間を要するので pods が running 状態になってから数分まって web に接続してください。

# plugin install
pod名取得
```
ns=dev-redmine
app=$(kubectl get pod --selector="app.kubernetes.io/name=redmine-app,environment=development" -n $ns | awk 'NR>1{print $1}' | head -1)
```
podに対して git clone コマンドを実行し、plugin を配置
```
kubectl -n $ns exec pod/${app} -- git clone https://github.com/jgraichen/redmine_dashboard.git /usr/src/redmine/plugins/redmine_dashboard/
```
pod を削除して再作成
```
kubectl delete -n $ns pod/${app}
```
