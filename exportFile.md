### 好些天没有来总结了，最近公司项目挺敢的，没有时间，现在项目差不多快弄完了，今天来总结一下
### 今天来和大家分享一下实际开放中，遇到导出文件，该注意的事项（把页面上的数据，调后台接口，下载下来） 2017.11.23 晚

```
    <div class="right">
        <img src="../../assets/export.png" alt="">
        <span @click="exportOrder()">导出订单</span>
    </div>


     exportOrder(){
        let formListData = {
          orderNo: this.form.orderNumber,  //订单号
          orderTimeS:  this.form.orderDate1 ? moment(this.form.orderDate1).format('YYYY-MM-DD HH:mm:ss') : '',  //下单日期开始
          orderTimeE: this.form.orderDate2 ? moment(this.form.orderDate2).format('YYYY-MM-DD HH:mm:ss') : '',  //下单日期结束
          payTimeS: this.form.paymentTime1 ? moment(this.form.paymentTime1).format('YYYY-MM-DD HH:mm:ss') : '',  //支付时间开始
          payTimeE: this.form.paymentTime2 ? moment(this.form.paymentTime2).format('YYYY-MM-DD HH:mm:ss') : '',  //支付时间结束
          orderAmoutMin: this.orderAmoutMin,   //订单金额小
          orderAmoutMax: this.orderAmoutMax,  //订单金额大
          orderStatus: this.form.orderStatus ? this.form.orderStatus : null ,  //订单状态
          roomName: this.form.roomName,  //房型名称
          pindex: this.page,  //当前页码,从1开始
          count: 5  //每页笔数
        };
        //调用查询酒店订单列表接口
        this.$axios.post(this.apiUrl + '/System/OrderWebManage/OutHotelOrderDataExcel', formListData , {responseType: "arraybuffer"})
          .then((response) => {
            if(response.status === 200){
              console.log(response);
              let blob = new Blob([response.data], {type: "application/vnd.ms-excel"});
              let fileName = "酒店订单信息列表";
              let a = document.createElement("a");
              document.body.appendChild(a);
              a.download = fileName;
              a.href = URL.createObjectURL(blob);
              a.click();
            }
          })
      }
```

###  以上是截取了Vue项目中的一段代码，最主要看的就是调接口的那一块，加了{responseType: "arraybuffer"} 和 response里面的那一段，需要转化一下！