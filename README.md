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
## dashboard
pod名取得(overlay/dev)
```
ns=dev-redmine
app=$(kubectl get pod --selector="app.kubernetes.io/name=redmine-app,environment=development" -n $ns | awk 'NR>1{print $1}' | head -1)
```
pod名取得(overlay/prod)
```
ns=prod-redmine
app=$(kubectl get pod --selector="app.kubernetes.io/name=redmine-app,environment=production" -n $ns | awk 'NR>1{print $1}' | head -1)
```


podに対して git clone コマンドを実行し、plugin を配置
```
kubectl -n $ns exec pod/${app} -- git clone https://github.com/jgraichen/redmine_dashboard.git -b v2.12.2 /usr/src/redmine/plugins/redmine_dashboard/
```
```
kubectl -n $ns exec pod/${app} -- bash -c "bundle install --without development test"
```

## redmine-slack
pod名取得(overlay/dev)
```
ns=dev-redmine
app=$(kubectl get pod --selector="app.kubernetes.io/name=redmine-app,environment=development" -n $ns | awk 'NR>1{print $1}' | head -1)
```
pod名取得(overlay/prod)
```
ns=prod-redmine
app=$(kubectl get pod --selector="app.kubernetes.io/name=redmine-app,environment=production" -n $ns | awk 'NR>1{print $1}' | head -1)
```
podに対して git clone コマンドを実行し、plugin を配置
```
kubectl -n $ns exec pod/${app} -- git clone https://github.com/sciyoshi/redmine-slack.git /usr/src/redmine/plugins/redmine_slack/
```
```
kubectl -n $ns exec pod/${app} -- bash -c "bundle install"
```

# thema install
## RedmineThemeMaterrial
pod名取得(overlay/dev)
```
ns=dev-redmine
app=$(kubectl get pod --selector="app.kubernetes.io/name=redmine-app,environment=development" -n $ns | awk 'NR>1{print $1}' | head -1)
```
pod名取得(overlay/prod)
```
ns=prod-redmine
app=$(kubectl get pod --selector="app.kubernetes.io/name=redmine-app,environment=production" -n $ns | awk 'NR>1{print $1}' | head -1)
```

podに対して git clone コマンドを実行し、thema を配置
```
kubectl -n $ns exec pod/${app} -- git clone https://github.com/fraoustin/RTMaterial.git /usr/src/redmine/public/themes/RTMaterial/
```