https://github.com/golang/go/wiki/CodeReviewComments



gofmt 代码格式化

	gofmt下载:
	gofmt 在 go安装包中

	gofmt使用方法：
	# gofmt -l -w  test.go  // 单文件
	# gofmt -l -w /root/go_project  // 整个项目


goimports 修正import包的规范

goimports工具是Go官方提供的一种工具，它能够为我们自动格式化 Go 语言代码并对所有引入的包进行管理，包括自动增删依赖的包引用、将依赖包按字母序排序并分类。

	源码安装：
	https://github.com/golang/tools.git

	# 创建文件夹  
	mkdir $GOPATH/src/golang.org/x/ 
	# 进入文件夹 
	cd  $GOPATH/src/golang.org/x/
	# 下载源码 
	git clone https://github.com/golang/tools.git
	# 安装
	go install golang.org/x/tools/cmd/goimports


	goimports 使用方法：
	goimports -w file  // 单个文件
	goimports -w /root/go_project // 整个项目

注：加了-w　会修正import包然后将修正后的代码覆盖原始内容


