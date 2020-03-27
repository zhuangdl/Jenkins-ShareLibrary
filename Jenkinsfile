#!groovy

@Library('jenkinslib') _

def tools = new org.devops.tools()

String workspace = "/data/jenkins"

pipeline {
	agent { 
		node { label "build0"	//指定运行节点的标签或者名称，等同于agent { label build0}
	           customWorkspace "${workspace}"
		}
	}
	
    hello()
	
    //可以省略
    options {	
    	timestamps()	//日志会有时间
    	skipDefaultCheckout()	//删除隐式checkout scm语句
    	disableConcurrentBuilds()	//禁止并行
    	timeout(time: 1, unit:'HOURS')	//流水线超时时间1h
    }

    //阶段
    stages { //下面有多个stage
    	//下载代码	
    	stage('GetCode'){	//阶段名称
    		steps{	//步骤
    			timeout(time: 5, unit: 'MINUTES'){	//步骤超时时间
    					print('获取代码')
    			}
    		}
    	}
        
    	//构建
    	stage('Build'){	
    		steps{	
    			timeout(time: 20, unit: 'MINUTES'){	
                    echo "构建代码"
    			}
    		}
    	}

    	//代码扫描
    	stage('CodeScan'){	
    		steps{	
        		timeout(time: 20, unit: "MINUTES"){	
                    script{
                        print("代码扫描")
                        tools.printmes('this is my lib!')   // 要放在script下
                    }
                }
    		}
    	}
    }

    //构建后的操作
    post {
    	always {	//总是执行的代码
            echo "总是执行"
    	}
   	//currentBuild是一个全局变量，description是构建描述
    	success {	//成功后执行
    		script{
    			currentBuild.description ="构建成功"	
    		}
    	}

    	failure {	//失败后执行
    		script{
    			currentBuild.description ="构建失败"
    		}
    	}

    	aborted {	//取消后执行
    		script{
    			currentBuild.description ="构建取消"
    		}
    	}
    }
}
