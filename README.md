## 1.Reactコマンド
### 1-1.プロジェクト実行
```
npm start
```
実行した後、[http://localhost:3000]アクセス

### 1-2.プロジェクトビルド
```
npm run build
```
実行した後、`build`ディレクトリに生成される。 


## 2.Nginx
### 2-1.Nginxインストール
```
sudo apt install nginx -y
sudo systemctl status nginx
```

### 2-2.sites-availableに設定ファイル作成
```
cd /etc/nginx/sites-available
```
上記のディレクトリに行って、以下の設定ファイルを作成してください。

server {
    listen 80;
    server_name localhost;

    root ${reactプロジェックのビルドディレクトリ};
    index index.html;

    location / {
        try_files $uri /index.html;
    }

    error_page 404 /index.html;

    access_log /var/log/nginx/lswn-profile-access.log;
    error_log /var/log/nginx/lswn-profile-error.log;
}

※上記の設定ファイルは2-3に使用します。

### 2-3.Symbolic Link生成
```
sudo ln -s sudo ln -s /etc/nginx/sites-available/${2-2に生成したファイル名}　/etc/nginx/sites-enabled/
```

### 2-4.Nginx設定確認
```
sudo nginx -t
```

### 2-5.Nginx再開始
```
sudo systemctl restart nginx
```


## 3.Docker

### 3-1.イメールビルド
```
docker build -t my-react-app -f nginx/Dockerfile .
```

### 3-2.コンテナ実行
```
docker run -d -p 3000:80 my-react-app
docker run -d -p 3001:80 my-react-app
```