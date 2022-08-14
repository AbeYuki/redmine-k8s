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