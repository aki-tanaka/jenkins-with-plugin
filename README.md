# jenkins-with-plugin
外部ネットワークに繋がらない環境用に、プラグインをインストール済みのJenkinsのDockerイメージを作成する。  

## 構築方法

1. Jenkinsの公式ドキュメントに沿って、外部ネットワークに繋がる環境でDocker上でJenkinsを起動する。
2. セットアップページで必要なプラグインを選択し、セットアップを行う。
3. `http://localhost:8080/script`にアクセス後、以下のスクリプトを実行し、インストールしたプラグインのリストを取得する。
   ```script
   Jenkins.instance.pluginManager.plugins.each{
     plugin -> 
       println ("${plugin.getShortName()}:${plugin.getVersion()}")
   }
   ```
4. 出力した結果を`plugins.txt`に設定する。
5. Dockerのビルドを実行する。


## メモ

起動用のコマンドは以下の通り。  

```bash
docker run --name myjenkins -p 8080:8080 -p 50001:50001 --restart=on-failure --env JENKINS_SLAVE_AGENT_PORT=50001 tanakaaki/jenkins-with-plugin:1.0.0
```
   
