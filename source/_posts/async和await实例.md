---
layout: layout
title: async和await实例
date: 2019-09-12 17:15:26
tags: js, promise,async,await
---



- 在做小程序时候碰到的地狱回调,
  - 1.获取storage中的ant_auth_id(`注：getStorage是异步执行`)
  - 2.在getStorage`异步`的原因导致，ajax请求只能放到回调中执行
  - 3.在ajax的回调中进行另一个storage的获得needContract
  - 4.needContract的回调中根据返回值进行另外两个storage的获得
  - 5.最后两个storage的回调中分别执行另外的函数。。。。这样就造成了回调地狱

```js
my.getStorage({
  key: 'ant_auth_id',
  success: function(res) {
    that.setData({
      ant_auth_id: res.data
    })
    http.postJSON({
      url: '/order/authQueryStatus',
      data: {
        biz_no: that.data.ant_auth_id 
      },
      success: res => {
        my.removeStorage({
          key: 'ant_auth_id',
          success: function(){
          }
        });
        if(res.data.status) {
          my.hideLoading();
          if(true) {

            my.getStorage({
              key: 'needContract',
              success: (res) => {
                if(res.data == 1) {
                  my.getStorage({
                    key: 'createContractData',
                    success: (res) => {
                      let data = res.data;
                      that.createContract(data);

                    }
                  })
                }else {
                  my.getStorage({
                    key: 'orderConfirmData',
                    success: (res) => {
                      let data = res.data;
                      that.createOrder(data);
                    }
                  })
                }
                // my.removeStorage({
                //   key: 'needContract', // 缓存数据的key
                //   success: (res) => {
                //   },
                // });
              }
            })

          }else {
            my.showToast({
              type: 'fail',
              content: '认证失败',
              duration: 2000,
              success: () => {

              }
            });
            if(res.data.type == 'company') {
              my.navigateTo({
                url: '/pages/cart/companyAuth'
              });
            }else {
              my.navigateTo({
                url: '/pages/cart/personalAuth'
              });
            }

          }
        }else {

        }
      }
    })
  },
  fail: function(res){
  }
});
```

- 用await优化后的代码：

```js
var getCatch=(key)=>{
        return new Promise(function(resolve,reject){
            my.getStorage({
                key: key,
                success: (res) => {
                  resolve(res.data)
                }
            })
        })
    }
    var requestUrl = (url, data) => {
      return new Promise((resolve, reject) => {
        http.postJSON({
          url: '/order/authQueryStatus',
          data: {
            biz_no: data 
          },
          success: (res) => {
            resolve(res.data);
          }
        })
      })
    }
    async function getCounty(province){
        let result1 = await getCatch('ant_auth_id');
        let result2 = await requestUrl('/order/authQueryStatus', result1);
        if(result2.status && result2.data) {
          let result3 = await getCatch('needContract');
          if(result3 == 1) {
            let result4 = await getCatch('createContractData');
            this.createContract(result4);
          }else {
            let result4 = await getCatch('orderConfirmData');
            this.createOrder(result4); 
          }
        }else {
          my.showToast({
            type: 'fail',
            content: '认证失败',
            duration: 2000,
            success: () => {

            }
          });
          if(result2.type == 'company') {
            my.navigateTo({
              url: '/pages/cart/companyAuth'
            });
          }else {
            my.navigateTo({
              url: '/pages/cart/personalAuth'
            });
          }
        }
    }
```

