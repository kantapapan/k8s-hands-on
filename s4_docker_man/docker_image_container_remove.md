# Dockerイメージとコンテナの削除方法


イメージを削除するにはまずそのイメージに紐づくコンテナを削除している必要がありますが、forceオプションによる強制削除も可能です。

## コンテナの削除方法

### 動いているコンテナの確認

	$ docker ps
	CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

### 停止しているコンテナの確認

	$ docker ps -a
	CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
	f60487285325        hello-world         "/hello"            2 seconds ago       Exited (0) 2 seconds ago                        nostalgic_goldstine
	8541933cccdc        hello-world         "/hello"            3 seconds ago       Exited (0) 2 seconds ago                        sick_euclid
	5200866fb18d        hello-world         "/hello"            4 seconds ago       Exited (0) 4 seconds ago                        hopeful_mestorf
	eea5b2620e02        hello-world         "/hello"            7 minutes ago       Exited (0) 7 minutes ago                        ecstatic_hopper
	a403ffe73d31        hello-world         "/hello"            16 minutes ago      Exited (0) 16 minutes ago                       romantic_thompson
			       romantic_thompson

### コンテナの削除

- コマンド： docker rm [コンテナID]
- 実際に削除してみる（一番古いコンテナ)

	$ docker rm a403ffe73d31
	a403ffe73d31

- 消えていることを確認

	$ docker ps -a
	CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
	f60487285325        hello-world         "/hello"            45 seconds ago      Exited (0) 44 seconds ago                       nostalgic_goldstine
	8541933cccdc        hello-world         "/hello"            46 seconds ago      Exited (0) 45 seconds ago                       sick_euclid
	5200866fb18d        hello-world         "/hello"            47 seconds ago      Exited (0) 46 seconds ago                       hopeful_mestorf
	eea5b2620e02        hello-world         "/hello"            8 minutes ago       Exited (0) 8 minutes ago                        ecstatic_hopper

- 複数指定もできるらしい

	$ docker rm eea5b2620e02 5200866fb18d
	eea5b2620e02
	5200866fb18d

- docker ps -a -q は、コンテナIDの一覧をだしてくれるので、それと合わせると一括削除もできる

	$ docker rm `docker ps -a -q`
	$ docker ps -a
	CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

## イメージの削除方法

### 現状のコンテナの確認

	$ docker ps -a
	CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                      PORTS               NAMES
	b07c7c83b892        docker-whale        "/bin/sh -c '/usr/gam"   26 seconds ago      Exited (0) 25 seconds ago                       clever_ptolemy

### 現状のイメージの確認

	$ docker images
	REPOSITORY          TAG                 IMAGE ID            CREATED              VIRTUAL SIZE
	docker-whale        latest              2f37bab81128        About a minute ago   274.1 MB
	hello-world         latest              0a6ba66e537a        2 weeks ago          960 B
	docker/whalesay     latest              ded5e192a685        5 months ago         247 MB

### イメージの削除

コマンド： docker rmi [イメージID]
実際にdocker-whale削除しようと実行してみるとコンテナが存在しているというメッセージが表示される

	$ docker rmi 2f37bab81128
	Error response from daemon: Conflict, cannot delete 2f37bab81128 because the container b07c7c83b892 is using it, use -f to force
	Error: failed to remove images: [2f37bab81128]

forceオプションをつけてみると消えた（コンテナが先に消えていればforceオプションは不要）
ちなみに、なんでこんなにいっぱいのファイルが削除されるんだろう？と思ったら中間ファイルらしい。
docker images -aで中間ファイルが確認できる。

	$ docker rmi -f 2f37bab81128 
	Untagged: docker-whale:latest
	Deleted: 2f37bab81128991f9e024bc1064be806cd3bf591e8d269d9ceea8f4f768b414e
	Deleted: 7191e8874482e349e2fb04ccb4c15b925439a8c884e3a266bd41adb324dd9f9a
	Deleted: e36145689a651426e87b3d906273c2c06aacd470d0953061f8d9bc00015d9d5c
	Deleted: f71b492d24edf7b8e065e2debc0f2eb6502524f8d5f8eb6bb89b2eacd187c2cf

削除されたか確認

	$ docker images
	REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
	hello-world         latest              0a6ba66e537a        2 weeks ago         960 B
	docker/whalesay     latest              ded5e192a685        5 months ago        247 MB

forceでイメージを削除した場合でも、コンテナのほうは残っているみたい

	$ docker ps -a

