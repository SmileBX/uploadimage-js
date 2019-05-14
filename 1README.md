# -js
使用步骤：
第一步：
  import {pathToBase64} from "@/utils/image-tools";  ----这个js文件放在until文件夹下面的
//上传参考图片
      chosseImg(n) {
      const that = this;
      console.log(n)
      let num = 0;
      if (that.prolist[n].referencePicList.length < that.imgLenght) {
        num = that.imgLenght - that.prolist[n].referencePicList.length;
        console.log(num,"最大图片数量")
        wx.chooseImage({
          count: num, //最大图片数量=最大数量-临时路径的数量
          sizeType: ["compressed"], //图片尺寸 original--原图；compressed--压缩图
          sourceType: ["album", "camera"], //选择图片的位置 album--相册选择, 'camera--使用相机
          success: res => {
            const imgPathArr = that.prolist[n].referencePicList
            that.prolist[n].referencePicList = []
            that.prolist[n].referencePicList = imgPathArr.concat(res.tempFilePaths);
                    console.log(res.tempFilePaths,'base')
                    console.log(that.prolist[n].referencePicList,'that.prolist[n].referencePicList')
            that.updateImg(n)
          }
        });
      }
    },
    // 更新图片数据
    updateImg(n){
      const that = this;
      console.log(that.prolist[n].referencePicList.length,'that.prolist[n].referencePicList1111111111')
        // 判断是否大于图片最大数量
        if (that.prolist[n].referencePicList.length === that.imgLenght*1) {
          that.prolist[n].isShowBtnUpload = false;
        }else{
          that.prolist[n].isShowBtnUpload = true;
        }
        let imgBase =[]
        // 根据临时路径数组imgPathArr获取base64图片
        for (let i = 0; i < that.prolist[n].referencePicList.length; i++) {
          pathToBase64(that.prolist[n].referencePicList[i]).then(res => {
           that.prolist[n].imgBase.push({
              PicUrl: res
            });
            console.log("gffffffffff");
            console.log(that.prolist[n].imgBase);
        })
          // wx.getFileSystemManager().readFile({
          //   filePath: (this.prolist[n].referencePicList)[i], //选择图片返回的相对路径
          //   encoding: "base64", //编码格式
          //   success: res => {
          //     //成功的回调
          //     imgBase.push({
          //       PicUrl: "data:image/png;base64," + res.data.toString()
          //     });
          
          //   }
          // });
        }
        that.prolist[n].imgBase = imgBase
    },
    deleteImg(i,n) {
      this.prolist[n].imgBase.splice(i, 1);
      this.prolist[n].referencePicList.splice(i, 1);
      if (this.prolist[n].referencePicList.length < this.imgLenght*1) {
        this.prolist[n].isShowBtnUpload = null;
        this.prolist[n].isShowBtnUpload = true;
      }
      this.updateImg(n)
    },
