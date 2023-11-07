
本地部署报错记录：

1、前端启动 npm run dev 报错如下

opensslErrorStack: [ 'error:03000086:digital envelope routines::initialization error' ],
  library: 'digital envelope routines',
  reason: 'unsupported',
  code: 'ERR_OSSL_EVP_UNSUPPORTED'
}

解决：
在启动脚本 package.json 中 加入环境变量  set NODE_OPTIONS=--openssl-legacy-provider &&
改动后如下 

  "scripts": {
    "serve": "set NODE_OPTIONS=--openssl-legacy-provider && vue-cli-service serve",
    "build": "set NODE_OPTIONS=--openssl-legacy-provider && vue-cli-service build",
    "lint": "set NODE_OPTIONS=--openssl-legacy-provider && vue-cli-service lint"
  },

2、如果遇到请求后端服务报错 404
先排查后端服务接口是否正常，比如是否连接数据库失败？
然后排查是否跨域问题，如果是跨域问题，则进行路由的修改

  devServer: {
 ...
    proxy: {
      '/api': {
        target: 'http://localhost:8080/',
        ws: true,
        changeOrigin: true,
        pathRewrite: {
         // ^代表字符串开头,实际发送请求时,会把请求开头的/demo删除
         // 因为/demo并不是请求的一部分,只是个代理的标识
            "^/api": ""
        }
      }
    } 
