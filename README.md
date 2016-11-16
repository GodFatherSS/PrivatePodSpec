#私有pod配置

1. 在git服务器上创建一个仓库，名字暂成为PrivateSpec，此仓库等价于一个私有的Spec Repo库，即为众多pods库的集合
2. 执行命令添加你的私有Repo到你的CocoaPods中
	
	~~~ sh
	$ pod repo add PrivateRepo https://github.com/GodFatherSS/PrivateSpec.git
	~~~
	执行成功后可以在目录`/Users/shishu/.cocoapods/repos/`中看到PrivateRepo目录。
3. 在git服务器上再次创建一个仓库，此仓库用于存储个人书写的第三方库，名字暂称为BBQ
4. 在命令行中执行以下命令：

	~~~ sh
	$ cd /Users/Shishu/Desktop
	$ pod lib create BBQ 
	# 按照提示完成工程创建
	~~~
	该段命令为创建一个名为BBQ的静态库工程，其中会包含一个可用于测试的Demo。
5. 修改工程中.podspec文件，需要修改的有homepage，以及source，source_file指向自己所要提供的第三方库的源文件位置：

	~~~ ruby
	#
	# Be sure to run `pod lib lint BBQ.podspec' to ensure this is a
	# valid spec before submitting.
	#
	# Any lines starting with a # are optional, but their use is encouraged
	# To learn more about a Podspec see http://guides.cocoapods.org/syntax/podspec.html
	#
	
	Pod::Spec.new do |s|
	  s.name             = 'BBQ'
	  s.version          = '0.1.1'
	  s.summary          = 'A short description of BBQ.'
	
	# This description is used to generate tags and improve search results.
	#   * Think: What does it do? Why did you write it? What is the focus?
	#   * Try to keep it short, snappy and to the point.
	#   * Write the description between the DESC delimiters below.
	#   * Finally, don't worry about the indent, CocoaPods strips it!
	
	  s.description      = <<-DESC
	TODO: Add long description of the pod here.
	                       DESC
	
	  s.homepage         = 'https://github.com/GodFatherSS/BBQ'
	  # s.screenshots     = 'www.example.com/screenshots_1', 'www.example.com/screenshots_2'
	  s.license          = { :type => 'MIT', :file => 'LICENSE' }
	  s.author           = { 'Seth' => 'shishu@ehousechina.com' }
	  s.source           = { :git => 'https://github.com/GodFatherSS/BBQ.git', :tag => s.version.to_s }
	  # s.social_media_url = 'https://twitter.com/<TWITTER_USERNAME>'
	
	  s.ios.deployment_target = '8.0'
	
	  s.source_files = 'BBQ/Classes/**/*'
	  
	  # s.resource_bundles = {
	  #   'BBQ' => ['BBQ/Assets/*.png']
	  # }
	
	  # s.public_header_files = 'Pod/Classes/**/*.h'
	  # s.frameworks = 'UIKit', 'MapKit'
	  # s.dependency 'AFNetworking', '~> 2.3'
	end
	
	~~~
6. 完成编写BBQ第三方库之后，先验证你的podspec配置是否正确：

	~~~ sh
	$ pod lib lint BBQ.podspec --allow-warnings
	~~~
如看到`BBQ pass validattion`，则通过验证，继续执行以下几步，否则请按照错误提示修改你的podspec文件：
	
	~~~ sh
	$ git add .
	$ git commit -a -m 'v0.1.0'
	$ git tag -a 0.1.0 -m 'v0.1.0'
	$ git remote add origin `BBQ仓库地址`
	$ git pull origin master
	$ git push origin master --tags
	~~~
7.	执行命令，将该podspec推送到远程私有Spec Repo中
	
	~~~ sh
	$ pod repo push PrivateRepo BBQ.podspec --allow-warnings
	~~~
	成功后可以进入目录`/Users/shishu/.cocoapods/repos/PrivateRepo`，其中可以看到如下目录：
	
	~~~
	|- BBQ
	|	|- 0.1.0
	|	|	|- BBQ.podspec
	~~~
8. 项目中使用，配置podfile

	~~~ ruby
	source 'https://github.com/GodFatherSS/PrivateSpec.git'
	target 'BBQDemo' do
	  pod 'BBQ', '~>0.1.1'
	end
	
	~~~


###其他
如果想添加到官方cocoapods中可搜索`pod trunk push ...`
